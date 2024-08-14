# clickhouse指南

## join优化
在 ClickHouse 中，分布式表（Distributed Tables）是用于分布式集群中的一种表类型，它将数据分布在多个节点上。当你需要在分布式表之间进行 JOIN 操作时，为了获得最佳性能，必须考虑数据的分布方式、分片键、查询优化等多个因素  

### 使用 GLOBAL 关键字进行跨节点 JOIN
当两个表的分布键不同，或者一个表的大小相对较小时，可以使用 GLOBAL 关键字来优化 JOIN 操作。
#### GLOBAL JOIN
当 JOIN 的一个表相对较小时，可以使用 GLOBAL 关键字将小表广播到所有节点上，以减少跨节点的 JOIN 操作。GLOBAL 会强制将小表的数据复制到每个分片上，从而在本地完成 JOIN。
```sql
SELECT ...
FROM orders_distributed AS o
JOIN GLOBAL customers_distributed AS c ON o.customer_id = c.customer_id
```
这种方式在小表和大表 JOIN 时非常有效，可以显著减少数据传输量。  

### 使用 JOIN 关键字的优化
在 ClickHouse 中，JOIN 的默认方式会在执行前将整个表加载到内存中，对于大表可能会导致内存消耗过高。通过指定合适的 JOIN 类型和优化参数，可以改善性能。
- ANY JOIN: 对于 JOIN 的表，如果确定右表只有一条匹配记录，使用 ANY JOIN 可以节省内存。
```sql
SELECT ...
FROM orders_distributed AS o
ANY LEFT JOIN customers_distributed AS c ON o.customer_id = c.customer_id
```
- ALL JOIN: 在需要匹配多条记录的场景下，可以使用 ALL JOIN。这会保留所有匹配的行。
- SEMI JOIN: 如果你只关心是否存在匹配项，可以使用 SEMI JOIN。
```sql
SELECT ...
FROM orders_distributed AS o
SEMI LEFT JOIN customers_distributed AS c ON o.customer_id = c.customer_id
```
- max_bytes_before_external_join参数：如果 JOIN 操作的数据量过大，可以通过设置 max_bytes_before_external_join 参数将部分数据写入磁盘，避免内存不足。
```sql
SET max_bytes_before_external_join = '1G';
```

### Colocate JOIN
ClickHouse 中的 Colocate JOIN 是一种优化分布式表 JOIN 操作的方法。当两个分布式表使用相同的分布键和分片策略时，ClickHouse 可以在同一个分片内进行 JOIN 操作，从而避免跨分片的数据传输。这种方式称为 Colocate JOIN。
#### 使用 Colocate JOIN 的条件
为了使用 Colocate JOIN，需要满足以下条件：
1. 相同的分布键：两个参与 JOIN 操作的分布式表必须使用相同的分布键（即 sharding key）。

2. 相同的分片数量：两个表的分片数量必须相同，这样可以保证相同的分片键值对会落在相同的分片上。

3. 相同的分布策略：两个表的分片策略（如分布函数、分片逻辑等）必须一致。
#### Colocate JOIN 的优点
- **减少数据传输**：因为 Colocate JOIN 保证了在同一分片内完成 JOIN 操作，所以避免了分片之间的数据传输，降低了网络开销。
- **提高查询性能**: 通过减少跨分片的数据传输，可以显著提高查询的性能，特别是在大规模数据集的场景下。
#### 如何实现 Colocate JOIN
要实现 Colocate JOIN，只需确保两个分布式表满足上述条件。ClickHouse 会自动检测这些条件，并在可能的情况下执行 Colocate JOIN。
##### 创建分布式表的示例：
假设有两个表 orders 和 customers，我们希望在 customer_id 上进行 JOIN 操作。
```sql
CREATE TABLE orders_local ON CLUSTER 'cluster_name' (
    order_id UInt32,
    customer_id UInt32,
    order_amount Float32,
    order_date Date
) ENGINE = MergeTree()
ORDER BY order_id
PARTITION BY toYYYYMM(order_date);

CREATE TABLE customers_local ON CLUSTER 'cluster_name' (
    customer_id UInt32,
    customer_name String,
    customer_address String
) ENGINE = MergeTree()
ORDER BY customer_id;

-- 创建分布式表，使用相同的分片键和分片数量
CREATE TABLE orders_distributed ON CLUSTER 'cluster_name' AS orders_local
ENGINE = Distributed('cluster_name', 'db_name', 'orders_local', customer_id);

CREATE TABLE customers_distributed ON CLUSTER 'cluster_name' AS customers_local
ENGINE = Distributed('cluster_name', 'db_name', 'customers_local', customer_id);

SELECT 
    o.order_id,
    o.order_date,
    o.order_amount,
    c.customer_name,
    c.customer_address
FROM 
    orders_distributed AS o
JOIN 
    customers_local AS c
ON 
    o.customer_id = c.customer_id;
```