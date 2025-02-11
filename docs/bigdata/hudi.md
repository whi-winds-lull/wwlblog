# hudi

## 一、指南

### 1、Clean命令

```
spark-submit \
--master yarn \
--driver-memory 8G \
--executor-memory 12G \
--executor-cores 6 \
--num-executors 15 \
--quque spark \
--conf 'spark.network.timeout=10000000' \
--conf 'spark.executor.heartbeatInterval=10000000' \
--conf 'spark.kryoserializer.buffer.max=512m' \
--conf 'spark.shuffle.memoryFraction=0.4' 
--class org.apache.hudi.utilities.HoodieCleaner xxx/hudi-utilities-bundle_2.12-0.11.1.jar \
--target-base-path xxx \
--hoodie-conf hoodie.cleaner.policy=KEEP_LATEST_FILE_VERSIONS \
--hoodie-conf hoodie.cleaner.fileversions.retained=1 \
--hoodie-conf hoodie.cleaner.parallelism=1200
```

### 2、cluster命令

```
vi /clusteringjob_partition.properties
clustering.plan.partition.filter.mode=SELECTED_PARTITIONS
hoodie.clustering.plan.strategy.cluster.begin.partition=2023-02-27
hoodie.clustering.plan.strategy.cluster.end.partition=2023-02-27
hoodie.clustering.plan.strategy.target.file.max.bytes=1073741824
hoodie.clustering.plan.strategy.small.file.limit=629145600

# HoodieClusteringJob
spark-submit \
--master yarn \
--driver-memory 8G \
--executor-memory 12G \
--executor-cores 6 \
--num-executors 15 \
--queue spark \
--conf 'spark.network.timeout=10000000' \
--conf 'spark.executor.heartbeatInterval=10000000' \
--conf 'spark.kryoserializer.buffer.max=512m' \
--conf 'spark.shuffle.memoryFraction=0.4' \
--class org.apache.hudi.utilities.HoodieClusteringJob xxx/hudi-utilities-bundle_2.12-0.11.1.jar \
--props xxx/clusteringjob_partition.properties \
--mode scheduleAndExecute \
--base-path xxx \
--table-name xxx \
--spark-memory 12g

# HoodieDeltaStreamer
spark-submit \
--master yarn \
--class org.apache.hudi.utilities.deltastreamer.HoodieDeltaStreamer \
xxx/hudi-utilities-bundle_2.12-0.11.1.jar \
--props xxx/clusteringjob.properties  \
--schemaprovider-class org.apache.hudi.utilities.schema.SchemaRegistryProvider \
--source-class org.apache.hudi.utilities.sources.AvroKafkaSource \
--source-ordering-field impresssiontime \
--table-type COPY_ON_WRITE \
--target-base-path xxx/ \
--target-table xxx \
--op INSERT \
--hoodie-conf hoodie.clustering.async.enabled=true \
--continuous
```

### 3、hudi-cli用法

```
1、引入obs
export CLIENT_JAR=xxx/hadoop-3.2.3/share/hadoop/tools/lib/hadoop-huaweicloud-3.2.3-hw-46.jar
2、进入
./hudi-cli.sh
3、连接表
connect --path xxx
4、对表进行操作
```

### 4、Hudi的Payload

Hudi 的Payload是一种可扩展的数据处理机制，通过不同的Payload我们可以实现复杂场景的定制化数据写入方式，大大增加了数据处理的灵活性。Hudi
Payload在写入和读取Hudi表时对数据进行去重、过滤、合并等操作的工具类，通过使用参数 "hoodie.datasource.write.payload.class"
指定我们需要使用的Payload class。

##### OverwriteWithLatestAvroPayload

- 这是默认的记录 Payload 实现。它通过调用预合并键（precombine key）的值上的 `.compareTo()`
  方法来确定最大值，并在合并时简单地选择最新的记录。这提供了“最后写入获胜”（latest-write-wins）的语义。

##### **DefaultHoodieRecordPayload**

`OverwriteWithLatestAvroPayload` 是基于排序字段进行预合并，并在合并时选择最新的记录，而 `DefaultHoodieRecordPayload`
则在预合并和合并时都遵循排序字段。我们通过一个示例来理解它们的区别：

假设排序字段是 `ts`，记录键是 `id`，模式（schema）如下：

```json
{
[
  {
    "name": "id",
    "type": "string"
  },
  {
    "name": "ts",
    "type": "long"
  },
  {
    "name": "name",
    "type": "string"
  },
  {
    "name": "price",
    "type": "string"
  }
]
}
```

存储中的当前记录：

| id | ts | name   | price   |
|:---|:---|:-------|:--------|
| 1  | 2  | name_2 | price_2 |

传入的记录：

| id | ts | name   | price   |
|:---|:---|:-------|:--------|
| 1  | 1  | name_1 | price_1 |

使用 `OverwriteWithLatestAvroPayload` 合并后的结果（最后写入获胜）：

| id | ts | name   | price   |
|:---|:---|:-------|:--------|
| 1  | 1  | name_1 | price_1 |

使用 `DefaultHoodieRecordPayload` 合并后的结果（始终遵循排序字段）：

| id | ts | name   | price   |
|:---|:---|:-------|:--------|
| 1  | 2  | name_2 | price_2 |

##### **EventTimeAvroPayload**

- 某些用例需要按事件时间（event time）合并记录，因此事件时间在这里充当排序字段。这种 Payload 特别适用于处理延迟到达的数据。对于此类用例，用户需要配置
  Payload 的事件时间字段。

##### **OverwriteNonDefaultsWithLatestAvroPayload**

- 这种 Payload 与 `OverwriteWithLatestAvroPayload`
  非常相似，但在合并记录时略有不同。在预合并时，它像 `OverwriteWithLatestAvroPayload`
  一样，基于排序字段选择某个键的最新记录。而在合并时，它只会覆盖存储中现有记录的指定字段，且仅当这些字段的值不等于默认值时才会覆盖。

##### **PartialUpdateAvroPayload**

- 这种 Payload
  支持部分更新。通常情况下，合并步骤会决定选择哪条记录，然后存储中的记录会被完全替换为所选的记录。但在某些情况下，需求是只更新某些字段，而不是替换整个记录。这称为部分更新。`PartialUpdateAvroPayload`
  为这种用例提供了开箱即用的支持。我们通过一个简单的示例来说明：

假设排序字段是 `ts`，记录键是 `id`，模式（schema）如下：

```json
{
[
  {
    "name": "id",
    "type": "string"
  },
  {
    "name": "ts",
    "type": "long"
  },
  {
    "name": "name",
    "type": "string"
  },
  {
    "name": "price",
    "type": "string"
  }
]
}
```

存储中的当前记录：

| id | ts | name   | price |
|:---|:---|:-------|:------|
| 1  | 2  | name_1 | null  |

传入的记录：

| id | ts | name | price   |
|:---|:---|:-----|:--------|
| 1  | 1  | null | price_1 |

使用 `PartialUpdateAvroPayload` 合并后的结果：

| id | ts | name   | price   |
|:---|:---|:-------|:--------|
| 1  | 2  | name_1 | price_1 |

### **核心区别**

| **Payload 类型**                                | **覆盖逻辑**                                   | **默认值处理**                                     |
|:----------------------------------------------|:-------------------------------------------|:----------------------------------------------|
| **OverwriteNonDefaultsWithLatestAvroPayload** | 只覆盖存储中记录的**非默认值字段**（如非 `null` 或非 `0` 的字段）。 | 如果传入记录的字段值为默认值（如 `null` 或 `0`），则不会覆盖存储中的对应字段。 |
| **PartialUpdateAvroPayload**                  | 只覆盖传入记录中**非空字段**（即不为 `null` 的字段）。          | 如果传入记录的字段值为 `null`，则不会覆盖存储中的对应字段。             |

## 二、问题

1、spark-sql建表和使用代码建表默认配置不一样，会导致写入数据重复

```
spark-sql默认'hoodie.table.keygenerator.class'= 'org.apache.hudi.keygen.ComplexKeyGenerator'

代码建表默认'hoodie.table.keygenerator.class'= 'org.apache.hudi.keygen.SimpleKeyGenerator'
```

2、任务停止或重启后可能会产生4KB的垃圾文件

```
# 查看
hadoop fs -ls -R xxx/receive_date=2023-05-22 | awk '{print $5, $8, $6, $7}' | sort -n | head -n 30
# 删除
hadoop fs -ls xxx/receive_date=2023-05-22 | awk '{if($5==4) print $8}' | xargs -I {} hadoop fs -rm {}
```

3、NoSuchMethodError from HBase when using Hudi with metadata table on HDFS?

```
NoSuchMethodError from HBase when using Hudi with metadata table on HDFS?
hudi里的hbase是由hadoop编译，如果使用hadoop3+hdfs会报错
解决：
(1) Download HBase source code from https://github.com/apache/hbase;
(2) Switch to the source code of 2.4.9 release with the tag rel/2.4.9:
git checkout rel/2.4.9
(3) Package a new version of HBase 2.4.9 with Hadoop 3 version:
mvn clean install -Denforcer.skip -DskipTests -Dhadoop.profile=3.0 -Psite-install-step
(4)Package Hudi again.
mvn install -DskipTests -Drat.skip=true -Pflink-bundle-shade-hive3 -Pflink1.13 -Dscala-2.12 -Dspark3.2
```