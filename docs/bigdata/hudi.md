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