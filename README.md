# cassandra-prometheus-jmx

Grafana dashboad can be found here: https://grafana.com/dashboards/5408

How to quickly set-up everything: https://www.robustperception.io/monitoring-cassandra-with-prometheus/

Metrics description:
 - https://medium.com/@mlowicki/cassandra-metrics-and-their-use-in-grafana-1f0dc33f9cca
 - https://www.datadoghq.com/blog/how-to-monitor-cassandra-performance-metrics/
 - https://blog.pythian.com/guide-to-cassandra-thread-pools/
 - https://www.datadoghq.com/blog/how-to-collect-cassandra-metrics/
 - https://aryanet.com/blog/cassandra-garbage-collector-tuning

Prometheus JMX exporter config:
```---
lowercaseOutputName: true
lowercaseOutputLabelNames: true
whitelistObjectNames: [
"org.apache.cassandra.metrics:type=ColumnFamily,name=ReadLatency,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=WriteLatency,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=RangeLatency,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=LiveSSTableCount,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=SSTablesPerReadHistogram,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=SpeculativeRetries,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=MemtableOnHeapSize,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=MemtableSwitchCount,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=MemtableLiveDataSize,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=MemtableColumnsCount,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=MemtableOffHeapSize,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=BloomFilterFalsePositives,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=BloomFilterFalseRatio,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=BloomFilterDiskSpaceUsed,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=BloomFilterOffHeapMemoryUsed,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=SnapshotsSize,*",
"org.apache.cassandra.metrics:type=ColumnFamily,name=TotalDiskSpaceUsed,*",
"org.apache.cassandra.metrics:type=CQL,name=RegularStatementsExecuted,*",
"org.apache.cassandra.metrics:type=CQL,name=PreparedStatementsExecuted,*",
"org.apache.cassandra.metrics:type=Compaction,name=PendingTasks,*",
"org.apache.cassandra.metrics:type=Compaction,name=CompletedTasks,*",
"org.apache.cassandra.metrics:type=Compaction,name=BytesCompacted,*",
"org.apache.cassandra.metrics:type=Compaction,name=TotalCompactionsCompleted,*",
"org.apache.cassandra.metrics:type=ClientRequest,name=Latency,*",
"org.apache.cassandra.metrics:type=ClientRequest,name=TotalLatency,*",
"org.apache.cassandra.metrics:type=ClientRequest,name=Unavailables,*",
"org.apache.cassandra.metrics:type=ClientRequest,name=Timeouts,*",
"org.apache.cassandra.metrics:type=Storage,name=Exceptions,*",
"org.apache.cassandra.metrics:type=Storage,name=TotalHints,*",
"org.apache.cassandra.metrics:type=Storage,name=TotalHintsInProgress,*",
"org.apache.cassandra.metrics:type=Storage,name=Load,*",
"org.apache.cassandra.metrics:type=Connection,name=TotalTimeouts,*",
"org.apache.cassandra.metrics:type=ThreadPools,name=CompletedTasks,*",
"org.apache.cassandra.metrics:type=ThreadPools,name=PendingTasks,*",
"org.apache.cassandra.metrics:type=ThreadPools,name=ActiveTasks,*",
"org.apache.cassandra.metrics:type=ThreadPools,name=TotalBlockedTasks,*",
"org.apache.cassandra.metrics:type=ThreadPools,name=CurrentlyBlockedTasks,*",
"org.apache.cassandra.metrics:type=DroppedMessage,name=Dropped,*",
"org.apache.cassandra.metrics:type=Cache,scope=KeyCache,name=HitRate,*",
"org.apache.cassandra.metrics:type=Cache,scope=KeyCache,name=Hits,*",
"org.apache.cassandra.metrics:type=Cache,scope=KeyCache,name=Requests,*",
"org.apache.cassandra.metrics:type=Cache,scope=KeyCache,name=Entries,*",
"org.apache.cassandra.metrics:type=Cache,scope=KeyCache,name=Size,*",
"org.apache.cassandra.metrics:type=Client,name=connectedNativeClients,*",
"org.apache.cassandra.metrics:type=Client,name=connectedThriftClients,*",
]
rules:
  - pattern: org.apache.cassandra.metrics<type=(Connection|Streaming), scope=(\S*), name=(\S*)><>(Count|Value)
    name: cassandra_$1_$3
    labels:
      address: "$2"
  - pattern: org.apache.cassandra.metrics<type=(\S*)(?:, ((?!scope)\S*)=(\S*))?(?:, scope=(\S*))?,
      name=(\S*)><>(Count|Value)
    name: cassandra_$1_$5
    labels:
      "$1": "$4"
      "$2": "$3"```
      
Tested on:
 - Prometheus v2.2
 - Grafana v5.0
 - JMX exporter v0.3.0
 - Cassandra v3.11
