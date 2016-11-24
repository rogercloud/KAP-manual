## 生产环境推荐配置(适用于v2.2.x及之后版本)

KAP的配置文件包括几个部分：*kylin.properties*，*kylin_hive_conf.xml*，*kylin_job_conf.xml*，*kylin_job_conf_inmem.xml*。其中*kylin.properties*是KAP的主要配置参见，控制KAP的运行时行为，*kylin_hive_conf.xml*用于配置KAP与Hive交互的参数，*kylin_job_conf*用于配置KAP与Hadoop集群交互的参数，其中*kylin_job_conf_inmem.xml*用在in-memory构建算法，*kylin_job_conf.xml*用在layer构建算法。

以下推荐配置将按照集群的规模分类，系统性能可能受其它外部系统参数影响，这里推荐仅作为经验值。

*Sandbox*表示用于单机sandbox虚拟机测试的环境，2核，10GB内存，10GB硬盘

*Prod*表示生产环境推荐配置，通常至少由5个节点组成的Hadoop集群，单机32核，128GB内存，20TB硬盘。

### kylin.properties

| Properties Name                          | Sandbox    | Prod    |
| ---------------------------------------- | ---------- | ------- |
| kylin.storage.hbase.compression-codec    | none       | snappy  |
| kylin.storage.hbase.region-cut-gb                   | 1          | 5       |
| kylin.storage.hbase.hfile-size-gb                | 1          | 2       |
| kylin.storage.hbase.min-region-count             | 1          | 1       |
| kylin.storage.hbase.max-region-count             | 100        | 500     |
| kylin.job.max-concurrent-jobs           | 10         | 20      |
| kylin.engine.mr.yarn-check-interval-seconds | 10         |         |
| kylin.job.sampling-percentage  | 100        |         |
| kylin.engine.mr.reduce-input-mb | 100        | 500     |
| kylin.engine.mr.max-reducer-number   | 100        | 500     |
| kylin.engine.mr.mapper-input-rows    | 200000     | 1000000 |
| kylin.job.step.timeout                   | 7200       |         |
| kylin.cube.algorithm                     | auto       |         |
| kylin.cube.algorithm.layer-or-inmem-threshold      | 8          |         |
| kylin.cube.aggrgroup.max-combination     | 4096       |         |
| kylin.dictionary.max.cardinality         | 5000000    |         |
| kylin.snapshot.max-mb              | 300        |         |
| kylin.query.scan-threshold               | 10000000   |         |
| kylin.query.memory-budget-bytes                   | 3221225472 |         |
| kylin.storage.hbase.coprocessor-mem-gb           | 3          |         |
| kylin.query.derived-filter-translation-threshold        | 100        |         |


### kylin.properties for KAP Plus

| Properties Name                          | Sandbox | Prod  |
| ---------------------------------------- | ------- | ----- |
| kap.storage.columnar.spark-conf.spark.driver.memory | 512m    | 8192m |
| kap.storage.columnar.spark-conf.spark.executor.memory | 512m    | 4096m |
| kap.storage.columnar.spark-conf.spark.yarn.am.memory | 512m    | 4096m |
| kap.storage.columnar.spark-conf.spark.executor.cores | 1       | 5     |
| kap.storage.columnar.spark-conf.spark.executor.instances | 1       | 4     |
| kap.storage.columnar.page-compression |         | SNAPPY     |
| kap.storage.columnar.ii-spill-threshold-mb |128         | 512     |




### kylin_hive_conf.xml

| Properties Name                          | Sandbox   | Prod                                     |
| ---------------------------------------- | --------- | ---------------------------------------- |
| dfs.replication                          | 2         |                                          |
| hive.exec.compress.output                | true      |                                          |
| hive.auto.convert.join.noconditionaltask | true      |                                          |
| hive.auto.convert.join.noconditionaltask.size | 100000000 |                                          |
| mapreduce.map.output.compress.codec      | N/A       | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.codec | N/A       | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK     |                                          |
| mapreduce.job.split.metainfo.maxsize     | -1        |                                          |

### kylin_job_conf.xml

| Properties Name                          | Sandbox | Prod                                     |
| ---------------------------------------- | ------- | ---------------------------------------- |
| mapreduce.job.split.metainfo.maxsize     | -1      |                                          |
| mapreduce.map.output.compress            | true    |                                          |
| mapreduce.map.output.compress.codec      | N/A     | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress | true    |                                          |
| mapreduce.output.fileoutputformat.compress.codec | N/A     | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK   |                                          |
| mapreduce.job.max.split.locations        | 2000    |                                          |
| dfs.replication                          | 2       |                                          |
| mapreduce.task.timeout                   | 3600000 |                                          |

### kylin_job_conf_inmem.xml

| Properties Name                          | Sandbox   | Prod                                     |
| ---------------------------------------- | --------- | ---------------------------------------- |
| mapreduce.job.split.metainfo.maxsize     | -1        |                                          |
| mapreduce.map.output.compress            | true      |                                          |
| mapreduce.map.output.compress.codec      | N/A       | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress | true      |                                          |
| mapreduce.output.fileoutputformat.compress.codec | N/A       | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK     |                                          |
| mapreduce.job.max.split.locations        | 2000      |                                          |
| dfs.replication                          | 2         |                                          |
| mapreduce.task.timeout                   | 7200000   |                                          |
| mapreduce.map.memory.mb                  | 3072      | 4096                                     |
| mapreduce.map.java.opts                  | -Xmx2700m | -Xmx3700m                                |
| mapreduce.task.io.sort.mb                | 200       | 200                                      |


