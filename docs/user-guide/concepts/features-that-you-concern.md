# Features that You Concern

## Does GreptimeDB support logs or events?

Yes. Since v0.9.0, GreptimeDB treats all time series as contextual events with timestamps, and thus unifies the processing of metrics, logs, and events. It supports analyzing metrics, logs, and events with SQL, PromQL, and streaming with continuous aggregation.

Please read the [log user guide](/user-guide/logs/overview.md).

## Does GreptimeDB support updates?

Sort of, Please refer to the [update data](/user-guide/manage-data/overview.md#update-data) for more information.

## Does GreptimeDB support deletion?

Yes, it does. Please refer to the [delete data](/user-guide/manage-data/overview.md#delete-data) for more information.

## Can I set TTL or retention policy for different tables or measurements?

Of course, you can set TTL for every table when creating it:

```sql
CREATE TABLE IF NOT EXISTS temperatures(
  ts TIMESTAMP TIME INDEX,
  temperature DOUBLE DEFAULT 10,
) engine=mito with(ttl='7d');
```

The TTL of temperatures is set to be seven days. 

Since 0.8, the database level `TTL` is supported too.

```sql
CREATE DATABASE test with(ttl='7d');
```

You can refer to the TTL option of the database and table create statement [here](/reference/sql/create.md).

## What are the compression rates of GreptimeDB?

The answer is it depends.
GreptimeDB uses the columnar storage layout, and compresses time series data by best-in-class algorithms.
And it will select the most suitable compression algorithm based on the column data's statistics and distribution.
GreptimeDB will provide rollups that can compress data more compactly but lose accuracy.

Therefore, the data compression rate of GreptimeDB may be between 2 and several hundred times, depending on the characteristics of your data and whether you can accept accuracy loss.

## How does GreptimeDB address the high cardinality issue?

GreptimeDB resolves this issue by:

- **Sharding**: It distributes the data and index between different region servers. Read the [architecture](./architecture.md) of GreptimeDB.
- **Smart Indexing**: It doesn't create the inverted index for every tag mandatorily, but chooses a proper index type based on the tag column's statistics and query workload. Find more explanation in this [blog](https://greptime.com/blogs/2022-12-21-storage-engine-design#smart-indexing).
- **MPP**: Besides the indexing capability, the query engine will use the vectorize execution query engine to execute the query in parallel and distributed.

## Does GreptimeDB support continuous aggregate or downsampling?

Since 0.8, GreptimeDB added a new function called `Flow`, which is used for continuous aggregation.  Please read the [user guide](/user-guide/continuous-aggregation/overview.md).

## Can I store data in object storage in the cloud?

Yes, GreptimeDB's data access layer is based on [OpenDAL](https://github.com/apache/incubator-opendal), which supports most kinds of object storage services.
The data can be stored in cost-effective cloud storage services such as AWS S3 or Azure Blob Storage, please refer to storage configuration guide [here](./../operations/configuration.md#storage-options).

GreptimeDB also offers a fully-managed cloud service [GreptimeCloud](https://greptime.com/product/cloud) to help you manage data in the cloud.

## Does GreptimeDB have disaster recovery solutions?

Yes. Please refer to [disaster recovery](/user-guide/operations/disaster-recovery/overview.md).