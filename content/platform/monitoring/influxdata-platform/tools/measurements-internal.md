---
title: InfluxDB _internal measurements and fields
description: >
  Use and understand the InfluxDB _internal measurements statistics and field keys
  that monitor InfluxDB and InfluxDB Enterprise servers.
aliases:
  - /platform/monitoring/tools/measurements-internal/
menu:
  platform:
    name: InfluxDB _internal measurements
    parent: Other monitoring tools
    weight: 2
---

By default, InfluxDB generates internal metrics and saves to the `_internal` database.
Use these metrics to monitor InfluxDB and InfluxDB Enterprise and to create alerts to notify you when problems arise.

### Disable the `_internal` database in production
InfluxData does **not** recommend using the `_internal` database in a production cluster.
It creates unnecessary overhead, particularly for busy clusters, that can overload an already loaded cluster.
Metrics stored in the `_internal` database primarily measure workload performance
and should only be tested in non-production environments.

To disable the `_internal` database, set [`store-enabled`](/influxdb/latest/administration/config/#monitoring-settings-monitor)
to `false` under the `[monitor]` section of your **InfluxDB configuration file**.

```toml
# ...
[monitor]
  # ...
  # Whether to record statistics internally.
  store-enabled = false
  #...
```

### Store internal metrics in an external monitor
To monitor InfluxDB `_internal` metrics in a production cluster, use Telegraf
and the [`influxdb` input plugin](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/influxdb)
to capture these metrics from the InfluxDB `/debug/vars` endpoint and store them
in an external InfluxDB monitoring instance.
For more information, see [Configure a Watcher of Watchers](/platform/monitoring/influxdata-platform/external-monitor-setup/).

> **Note:** When using the "watcher of watcher (WoW)" configuration, InfluxDB
> metric field keys are prepended with `infuxdb_`, but are otherwise identical
to those listed [below](#influxdb-internal-measurements-and-fields).

## Visualize InfluxDB internal metrics
Use the [InfluxDB OSS Monitor dashboard](/platform/monitoring/influxdata-platform/monitoring-dashboards/#monitor-influxdb-oss)
or the [InfluxDB Enterprise Monitor dashboard](/platform/monitoring/influxdata-platform/monitoring-dashboards/#monitor-influxdb-enterprise)
to visualize InfluxDB `_internal` metrics.

## Metric types

#### Counter
A **counter** is a cumulative metric that represents a single monotonically increasing counter.
Counters only increase (except on reset).
All counters reset to zero when InfluxDB restarts.

#### Gauge
A **gauge** is a metric that represents a single numerical value that can arbitrarily go up or down.

## InfluxDB \_internal measurements and fields
{{% truncate %}}
- [ae](#ae-enterprise-only) (Enterprise only)
  - [bytesRx](#bytesrx)
  - [errors](#errors)
  - [jobs](#jobs)
  - [jobsActive](#jobsactive)
- [cluster](#cluster-enterprise-only) (Enterprise only)
  - [copyShardReq](#copyshardreq)
  - [createIteratorReq](#createiteratorreq)
  - [expandSourcesReq](#expandsourcesreq)
  - [fieldDimensionsReq](#fielddimensionsreq)
  - [iteratorCostReq](#iteratorcostreq)
  - [removeShardReq](#removeshardreq)
  - [writeShardFail](#writeshardfail)
  - [writeShardPointsReq](#writeshardpointsreq)
  - [writeShardReq](#writeshardreq)
- [cq](#cq)
  - [queryFail](#queryfail)
  - [queryOk](#queryok)
- [database](#database)
  - [numMeasurements](#nummeasurements)
  - [numSeries](#numseries)
- [hh](#hh-enterprise-only) (Enterprise only)
  - [writeShardReq](#writeshardreq)
  - [writeShardReqPoints](#writeshardreqpoints)
- [hh_database](#hh-database) (Enterprise only)
  - [bytesRead](#bytesread)
  - [bytesWritten](#byteswritten)
  - [queueBytes](#queuebytes)
  - [queueDepth](#queuedepth)
  - [writeBlocked](#writeblocked)
  - [writeDropped](#writedropped)
  - [writeNodeReq](#writenodereq)
  - [writeNodeReqFail](#writenodereqfail)
  - [writeNodeReqPoints](#writenodereqpoints)
  - [writeShardReq](#writeshardreq)
  - [writeShardReqPoints](#writeshardreqpoints)
- [hh_processor](#hh-processor-enterprise-only) (Enterprise only)
  - [bytesRead](#bytesread)
  - [bytesWritten](#byteswritten)
  - [queueBytes](#queuebytes)
  - [queueDepth](#queuedepth)
  - [writeBlocked](#writeblocked)
  - [writeDropped](#writedropped)
  - [writeNodeReq](#writenodereq)
  - [writeNodeReqFail](#writenodereqfail)
  - [writeNodeReqPoints](#writenodereqpoints)
  - [writeShardReq](#writeshardreq)
  - [writeShardReqPoints](#writeshardreqpoints)
- [httpd](#httpd)
  - [authFail](#authfail)
  - [clientError](#clienterror)
  - [pingReq](#pingreq)
  - [pointsWrittenDropped](#pointswrittendropped)
  - [pointsWrittenFail](#pointswrittenfail)
  - [pointsWrittenOK](#pointswrittenok)
  - [promReadReq](#promreadreq)
  - [promWriteReq](#promwritereq)
  - [fluxQueryReq](#fluxqueryreq)
  - [fluxQueryDurationNs](#fluxquerydurationns)
  - [queryReq](#queryreq)
  - [queryReqDurationNs](#queryreqdurationns)
  - [queryRespBytes](#queryrespbytes)
  - [recoveredPanics](#recoveredpanics)
  - [req](#req)
  - [reqActive](#reqactive)
  - [reqDurationNs](#reqdurationns)
  - [serverError](#servererror)
  - [statusReq](#statusreq)
  - [writeReq](#writereq)
  - [writeReqActive](#writereqactive)
  - [writeReqBytes](#writereqbytes)
  - [writeReqDurationNs](#writereqdurationns)
- [queryExecutor](#queryexecutor)
  - [queriesActive](#queriesactive)
  - [queriesExecuted](#queriesexecuted)
  - [queriesFinished](#queriesfinished)
  - [queryDurationNs](#querydurationns)
  - [recoveredPanics](#recoveredpanics)
- [rpc](#rpc-enterprise-only) (Enterprise only)
  - [idleStreams](#idlestreams)
  - [liveConnections](#liveconnections)
  - [liveStreams](#livestreams)
  - [rpcCalls](#rpccalls)
  - [rpcFailures](#rpcfailures)
  - [rpcReadBytes](#rpcreadbytes)
  - [rpcRetries](#rpcretries)
  - [rpcWriteBytes](#rpcwritebytes)
  - [singleUse](#singleuse)
  - [singleUseOpen](#singleuseopen)
  - [totalConnections](#totalconnections)
  - [totalStreams](#totalstreams)
- [runtime](#runtime)
  - [Alloc](#alloc)
  - [Frees](#frees)
  - [HeapAlloc](#heapalloc)
  - [HeapIdle](#heapidle)
  - [HeapInUse](#heapinuse)
  - [HeapObjects](#heapobjects)
  - [HeapReleased](#heapreleased)
  - [HeapSys](#heapsys)
  - [Lookups](#lookups)
  - [Mallocs](#mallocs)
  - [NumGC](#numgc)
  - [NumGoroutine](#numgoroutine)
  - [PauseTotalNs](#pausetotalns)
  - [Sys](#sys)
  - [TotalAlloc](#totalalloc)
- [shard](#shard)
  - [diskBytes](#diskbytes)
  - [fieldsCreate](#fieldscreate)
  - [seriesCreate](#seriescreate)
  - [writeBytes](#writebytes)
  - [writePointsDropped](#writepointsdropped)
  - [writePointsErr](#writepointserr)
  - [writePointsOk](#writepointsok)
  - [writeReq](#writereq)
  - [writeReqErr](#writereqerr)
  - [writeReqOk](#writereqok)
- [subscriber](#subscriber)
  - [createFailures](#createfailures)
  - [pointsWritten](#pointswritten)
  - [writeFailures](#writefailures)
- [tsm1_cache](#tsm1-cache)
  - [WALCompactionTimeMs](#walcompactiontimems)
  - [cacheAgeMs](#cacheagems)
  - [cachedBytes](#cachedbytes)
  - [diskBytes](#diskbytes)
  - [memBytes](#membytes)
  - [snapshotCount](#snapshotcount)
  - [writeDropped](#writedropped)
  - [writeErr](#writeerr)
  - [writeOk](#writeok)
- [tsm1_engine](#tsm1-engine)
  - [cacheCompactionDuration](#cachecompactionduration)
  - [cacheCompactionErr](#cachecompactionerr)
  - [cacheCompactions](#cachecompactions)
  - [cacheCompactionsActive](#cachecompactionsactive)
  - [tsmFullCompactionDuration](#tsmfullcompactionduration)
  - [tsmFullCompactionErr](#tsmfullcompactionerr)
  - [tsmFullCompactionQueue](#tsmfullcompactionqueue)
  - [tsmFullCompactions](#tsmfullcompactions)
  - [tsmFullCompactionsActive](#tsmfullcompactionsactive)
  - [tsmLevel1CompactionDuration](#tsmlevel1compactionduration)
  - [tsmLevel1CompactionErr](#tsmlevel1compactionerr)
  - [tsmLevel1CompactionQueue](#tsmlevel1compactionqueue)
  - [tsmLevel1Compactions](#tsmlevel1compactions)
  - [tsmLevel1CompactionsActive](#tsmlevel1compactionsactive)
  - [tsmLevel2CompactionDuration](#tsmlevel2compactionduration)
  - [tsmLevel2CompactionErr](#tsmlevel2compactionerr)
  - [tsmLevel2CompactionQueue](#tsmlevel2compactionqueue)
  - [tsmLevel2Compactions](#tsmlevel2compactions)
  - [tsmLevel2CompactionsActive](#tsmlevel2compactionsactive)
  - [tsmLevel3CompactionDuration](#tsmlevel3compactionduration)
  - [tsmLevel3CompactionErr](#tsmlevel3compactionerr)
  - [tsmLevel3CompactionQueue](#tsmlevel3compactionqueue)
  - [tsmLevel3Compactions](#tsmlevel3compactions)
  - [tsmLevel3CompactionsActive](#tsmlevel3compactionsactive)
  - [tsmOptimizeCompactionDuration](#tsmoptimizecompactionduration)
  - [tsmOptimizeCompactionErr](#tsmoptimizecompactionerr)
  - [tsmOptimizeCompactionQueue](#tsmoptimizecompactionqueue)
  - [tsmOptimizeCompactions](#tsmoptimizecompactions)
  - [tsmOptimizeCompactionsActive](#tsmoptimizecompactionsactive)
- [tsm1_filestore](#tsm1-filestore)
  - [diskBytes](#diskbytes)
  - [numFiles](#numfiles)
- [tsm1_wal](#tsm1-wal)
  - [currentSegmentDiskBytes](#currentsegmentdiskbytes)
  - [oldSegmentsDiskBytes](#oldsegmentsdiskbytes)
  - [writeErr](#writeerr)
  - [writeOk](#writeok)
- [write](#write)
  - [pointReq](#pointreq)
  - [pointReqHH](#pointreqhh-enterprise-only) (Enterprise only)
  - [pointReqLocal](#pointreqlocal-enterprise-only) (Enterprise only)
  - [pointReqRemote](#pointreqremote-enterprise-only) (Enterprise only)
  - [req](#req)
  - [subWriteDrop](#subwritedrop)
  - [subWriteOk](#subwriteok)
  - [writeDrop](#writedrop)
  - [writeError](#writeerror)
  - [writeOk](#writeok)
  - [writePartial](#writePartial-enterprise-only) (Enterprise only)
  - [writeTimeout](#writetimeout)
{{% /truncate %}}

### ae (Enterprise only)
The measurement statistics related to the Anti-Entropy (AE) engine in InfluxDB Enterprise clusters.

#### bytesRx
The number of bytes received by the data node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### errors
The total number of anti-entropy jobs that have resulted in errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### jobs
The total number of jobs executed by the data node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### jobsActive
The number of active (currently executing) jobs.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

---

### cluster (Enterprise only)
The `cluster` measurement tracks statistics related to the clustering features of the data nodes in InfluxDB Enterprise.
The tags on the series indicate the source host of the stat.

#### copyShardReq
The number of internal requests made to copy a shard from one data node to another.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### createIteratorReq
The number of read requests from other data nodes in the cluster.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### expandSourcesReq
The number of remote node requests made to find measurements on this node that match a particular regular expression.
Indicates a SELECT from a regex initiated on a different data node, which then sent an internal request to this node.
There is not currently a statistic tracking how many queries with a regex, instead of a fixed measurement, were initiated on a particular node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### fieldDimensionsReq
The number of remote node requests for information about the fields and associated types, and tag keys of measurements on this data node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### iteratorCostReq
The number of internal requests for iterator cost.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### removeShardReq
The number of internal requests to delete a shard from this data node.
Exclusively incremented by use of the `influxd-ctl remove shard` command.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardFail
The total number of internal write requests from a remote node that failed.
It's the cousin of InfluxDB shard stat `writeReqErr`.
A write request over HTTP is received by Node A. Node A does not have the shard locally,
so it creates an internal request to Node B instructing what to write and to which shard.
If Node B sees the request and if anything goes wrong, Node B increments its own `writeShardFail`.
Depending on what went wrong, in most circumstances Node B would also increment its `writeReqErr` stat inherited from InfluxDB OSS.
If Node A had the shard locally, there would be no internal request to write data
to a remote node, so `writeShardFail` would not be incremented.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardPointsReq
The number of points in every internal write request from any remote node, regardless of success.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardReq
The number of internal write requests from a remote data node, regardless of success.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### cq
The measurement statistics related to continuous queries (CQs).

#### queryFail
The total number of continuous queries that executed but failed.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queryOk
The total number of continuous queries that executed successfully.
Note that this value may be incremented in some cases where a CQ is initiated
but does not actually run, for example, due to misconfigured resample intervals.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### database

#### numMeasurements
The current number of measurements in the specified database.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### numSeries
The current series cardinality of the specified database.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

The series cardinality values are estimates, based on [HyperLogLog++ (HLL++)](https://github.com/influxdata/influxdb/blob/master/pkg/estimator/hll/hll.go).
The numbers returned by the estimates when there are thousands or millions of
measurements or series should be accurate within a relatively small margin of error.

---

### hh (Enterprise only)
The `hh` measurement statistics track events resulting in new hinted handoff (HH)
processors in InfluxDB Enterprise.
The `hh` measurement has one additional tag:

- `path` - The path to the durable hinted handoff queue on disk.

#### writeShardReq
The number of initial write requests handled by the hinted handoff engine for a remote node.
Subsequent write requests to this node, destined for the same remote node, do not increment this statistics.
This statistic resets to `0` upon restart of `influxd`, regardless of the state
the last time the process was alive. It is incremented when the HH "supersystem"
is instructed to enqueue a write for the node, and the "subsystem" for the destination
node doesn't exist and has to be created, and the "subsystem" created successfully.
If HH files are on disk for a remote node at process startup, the branch that
increments this stat will not be reached.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardReqPoints
The number of write requests for each point in the initial request to the hinted
handoff engine for a remote node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### hh_database (Enterprise only)
The `hh_database` measurement aggregates all hinted handoff queues for a single database and node.
This allows accurate reporting of total queue size for a single database to a target node.

The `hh_database` measurement has two additional tags:

- `db` — The name of the database
- `node` — The node identifier

#### bytesRead
The size, in bytes, of points read from the hinted handoff queue and sent to its destination data node.
Note that if the data node process is restarted while there is data in the HH queue,
`bytesRead` may settle to a number larger than `bytesWritten`.
Hinted handoff writes occur in concurrent batches as determined by the
[`retry-concurrency`](/enterprise_influxdb/latest/administration/configuration/#retry-concurrency-20) setting.
If an individual write succeeds, the metric is incremented.
If any write out of the whole batch fails, the entire batch is considered unsuccessful,
and every part of the batch will be retried later. This was not the intended behavior of this stat.
The other situation where `bytesRead` could be larger would be after a restart of the process.
Say at startup there were 1000 bytes still enqueued in HH from the previous run of the process.
Immediately after a restart, both `bytesRead` and `bytesWritten` are set to zero.
Assuming HH is properly depleted, and no future writes require HH, then the stats will read 1000 bytes read and 0 bytes written.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

>**Note:** Resets to zero after crash or restart, even if the HH queue was non-empty.

#### bytesWritten
The total number of bytes written to the hinted handoff queue.
Note that this statistic only tracks bytes written during the lifecycle of the current process.
Upon restart or a crash, this statistic resets to zero, even if the hinted handoff queue was not empty.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queueBytes
The total number of bytes remaining in the hinted handoff queue.
This statistic should accurately and absolutely track the number of bytes of encoded
data waiting to be sent to the remote node.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

This statistic should remain correct across restarts, unlike `bytesRead` and `bytesWritten` (see [#780](https://github.com/influxdata/docs.influxdata.com/issues/780)).

#### queueDepth
The total number of segments in the hinted handoff queue. The HH queue is a sequence of 10MB "segment" files.
This is a coarse-grained statistic that roughly represents the amount of data queued for a remote node.
The `queueDepth` values can give you a sense of when a queue is growing or shrinking.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### writeBlocked
The number of writes blocked because the number of concurrent HH requests exceeds the limit.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeDropped
The number of writes dropped from the HH queue because the write appeared to be corrupted.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeNodeReq
The total number of write requests that succeeded in writing a batch to the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeNodeReqFail
The total number of write requests that failed in writing a batch of data from the
hinted handoff queue to the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeNodeReqPoints
The total number of points successfully written from the HH queue to the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardReq
The total number of every write batch request enqueued into the hinted handoff queue.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardReqPoints
The total number of points enqueued into the hinted handoff queue.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### hh_processor (Enterprise only)
The `hh_processor` measurement stores statistics for a single queue (shard).
In InfluxDB Enterprise, there is a hinted handoff processor on each data node.

The `hh_processor` measurement has two additional tags:

- `node` - The destination node for the recorded metrics.
- `path` - The path to the durable hinted handoff queue on disk.

> **Note:** The `hh_processor` statistics against a host are only accurate for the lifecycle of the current process.
> If the process crashes or restarts, `bytesRead` and `bytesWritten` are reset to zero, even if the HH queue was non-empty.

#### bytesRead
The size, in bytes, of points read from the hinted handoff queue and sent to its destination data node.
Note that if the data node process is restarted while there is data in the HH queue,
`bytesRead` may settle to a number larger than `bytesWritten`.
Hinted handoff writes occur in concurrent batches as determined by the
[`retry-concurrency`](/enterprise_influxdb/latest/administration/configuration/#retry-concurrency-20) setting.
If an individual write succeeds, the metric is incremented.
If any write out of the whole batch fails, the entire batch is considered unsuccessful,
and every part of the batch will be retried later.
This was not the intended behavior of this stat.
The other situation where `bytesRead` could be larger would be after a restart of the process.
Say at startup there were 1000 bytes still enqueued in HH from the previous run of the process.
Immediately after a restart, both `bytesRead` and `bytesWritten` are set to zero.
Assuming HH is properly depleted, and no future writes require HH, then the stats
will read 1000 bytes read and 0 bytes written.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

>**Note:** Resets to zero after crash or restart, even if the HH queue was non-empty.

#### bytesWritten
The total number of bytes written to the hinted handoff queue.
Note that this statistic only tracks bytes written during the lifecycle of the current process.
Upon restart or a crash, this statistic resets to zero, even if the hinted handoff queue was not empty.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queueBytes
The total number of bytes remaining in the hinted handoff queue.
This statistic should accurately and absolutely track the number of bytes of encoded
data waiting to be sent to the remote node.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

This statistic should remain correct across restarts, unlike `bytesRead` and `bytesWritten`
(see [#780](https://github.com/influxdata/docs.influxdata.com/issues/780)).

#### queueDepth
The total number of segments in the hinted handoff queue. The HH queue is a sequence of 10MB "segment" files.
This is a coarse-grained statistic that roughly represents the amount of data queued for a remote node.
The `queueDepth` values can give you a sense of when a queue is growing or shrinking.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### writeBlocked
The number of writes blocked because the number of concurrent HH requests exceeds the limit.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeDropped
The number of writes dropped from the HH queue because the write appeared to be corrupted.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeNodeReq
The total number of write requests that succeeded in writing a batch to the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeNodeReqFail
The total number of write requests that failed in writing a batch of data from the
hinted handoff queue to the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeNodeReqPoints
The total number of points successfully written from the HH queue to the destination nod.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardReq
The total number of every write batch request enqueued into the hinted handoff queue.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeShardReqPoints
The total number of points enqueued into the hinted handoff queue.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### httpd
The `httpd` measurement stores fields related to the InfluxDB HTTP server.

#### authFail
The number of HTTP requests that were aborted due to authentication being required,
but not supplied or incorrect.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### clientError
The number of HTTP responses due to client errors, with a `4XX` HTTP status code.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### fluxQueryReq
The number of Flux query requests served.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### fluxQueryReqDurationNs
The duration (wall-time), in nanoseconds, spent executing Flux query requests.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pingReq
The number of times InfluxDB HTTP server served the `/ping` HTTP endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointsWrittenDropped
The number of points dropped by the storage engine.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointsWrittenFail
The number of points accepted by the HTTP `/write` endpoint, but unable to be persisted.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointsWrittenOK
The number of points accepted by the HTTP `/write` endpoint and persisted successfully.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### promReadReq
The number of read requests to the Prometheus `/read` endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### promWriteReq
The number of write requests to the Prometheus `/write` endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queryReq
The number of query requests.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queryReqDurationNs
The total query request duration, in nanosecond (ns).

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queryRespBytes
The total number of bytes returned in query responses.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### recoveredPanics
The total number of panics recovered by the HTTP handler.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### req
The total number of HTTP requests served.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### reqActive  
The number of currently active requests.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### reqDurationNs
The duration (wall time), in nanoseconds, spent inside HTTP requests.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### serverError
The number of HTTP responses due to server errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### statusReq
The number of status requests served using the HTTP `/status` endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReq
The number of write requests served using the HTTP `/write` endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReqActive
The number of currently active write requests.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReqBytes
The total number of bytes of line protocol data received by write requests, using the HTTP `/write` endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReqDurationNs
The duration (wall time), in nanoseconds, of write requests served using the `/write` HTTP endpoint.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### queryExecutor

The `queryExecutor` statistics related to usage of the Query Executor of the InfluxDB engine.

#### queriesActive
The number of active queries currently being handled.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

##### queriesExecuted
The number of queries executed (started).

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queriesFinished
The number of queries that have finished executing.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### queryDurationNs
The duration (wall time), in nanoseconds, of every query executed.
If one query took 1000 ns from start to finish, and another query took 500 ns
from start to finish and ran before the first query finished, the statistic
would increase by 1500.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### recoveredPanics
The number of panics recovered by the Query Executor.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### rpc (Enterprise only)

The `rpc` measurement statistics are related to the use of RPC calls within InfluxDB Enterprise clusters.

#### idleStreams
The number of idle multiplexed streams across all live TCP connections.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### liveConnections
The current number of live TCP connections to other nodes.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### liveStreams
The current number of live multiplexed streams across all live TCP connections.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### rpcCalls
The total number of RPC calls made to remote nodes.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### rpcFailures
The total number of RPC failures, which are RPCs that did not recover.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### rpcReadBytes
The total number of RPC bytes read.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### rpcRetries
The total number of RPC calls that retried at least once.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### rpcWriteBytes
The total number of RPC bytes written.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### singleUse
The total number of single-use connections opened using Dial.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### singleUseOpen
The number of single-use connections currently open.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### totalConnections
The total number of TCP connections that have been established.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### totalStreams
The total number of streams established.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### runtime
The `runtime` measurement statistics include a subset of MemStats records statistics about the Go memory allocator.
The  `runtime` statistics can be useful to determine poor memory allocation strategies and related performance issues.

The [Go runtime package](https://golang.org/pkg/runtime/) contains operations that
interact with Go's runtime system, including functions used to control goroutines.
It also includes the low-level type information used by the [Go reflect package](https://golang.org/pkg/reflect/).

#### Alloc
The currently allocated number of bytes of heap objects.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### Frees
The cumulative number of freed (live) heap objects.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### HeapAlloc
The size, in bytes, of all heap objects.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### HeapIdle
The number of bytes of idle heap objects.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### HeapInUse
The number of bytes in in-use spans.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### HeapObjects
The number of allocated heap objects.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### HeapReleased
The number of bytes of physical memory returned to the OS.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### HeapSys
The number of bytes of heap memory obtained from the OS.
Measures the amount of virtual address space reserved for the heap.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### Lookups
The number of pointer lookups performed by the runtime.
Primarily useful for debugging runtime internals.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### Mallocs
The total number of heap objects allocated.
The total number of live objects is [Frees](#frees).

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### NumGC
The number of completed GC (garbage collection) cycles.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### NumGoroutine
The total number of Go routines.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### PauseTotalNs
The total duration, in nanoseconds, of total GC (garbage collection) pauses.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### Sys
The total number of bytes of memory obtained from the OS.
Measures the virtual address space reserved by the Go runtime for the heap,
stacks, and other internal data structures.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### TotalAlloc
The total number of bytes allocated for heap objects.
This statistic does not decrease when objects are freed.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### shard
The `shard` measurement statistics are related to working with shards in InfluxDB OSS and InfluxDB Enterprise.

The shard measurement includes one additional tag:

- **indexType:** The type of index `inmem` or `tsi1`.

#### diskBytes
The size, in bytes, of the shard, including the size of the data directory and the WAL directory.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### fieldsCreate
The number of fields created.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### seriesCreate
Then number of series created.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeBytes
The number of bytes written to the shard.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writePointsDropped
The number of requests to write points t dropped from a write.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

Also, `http.pointsWrittentDropped` incremented when a point is dropped from a write
(see [#780](https://github.com/influxdata/docs.influxdata.com/issues/780)).

#### writePointsErr
The number of requests to write points that failed to be written due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writePointsOk
The number of points written successfully.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReq
The total number of write requests.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReqErr
The total number of write requests that failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeReqOk
The total number of successful write requests.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### subscriber
The `subscriber` measurement statistics are related to the usage of InfluxDB subscriptions.

#### createFailures
The number of subscriptions that failed to be created.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointsWritten  
The total number of points that were successfully written to subscribers.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeFailures  
The total number of batches that failed to be written to subscribers.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### tsm1_cache
The `tsm1_cache` measurement statistics are related to the usage of the TSM cache.
The following query example calculates various useful measurements related to the TSM cache.

```sql
SELECT
     max(cacheAgeMs) / 1000.000 AS CacheAgeSeconds,
     max(memBytes) AS MaxMemBytes, max(diskBytes) AS MaxDiskBytes,
     max(snapshotCount) AS MaxSnapShotCount,
     (last(cachedBytes) - first(cachedBytes)) / (last(WALCompactionTimeMs) - first(WALCompactionTimeMs)) - 1000.000 AS CompactedBytesPerSecond,
     last(cachedBytes) AS CachedBytes,
     (last(cachedBytes) - first(cachedBytes))/300 as CacheThroughputBytesPerSecond
FROM _internal.monitor.tsm1_cache
WHERE time > now() - 1h
GROUP BY time(5m), path
```

#### cacheAgeMs
The duration, in milliseconds, since the cache was last snapshotted at sample time.
This statistic indicates how busy the cache is.
Large numbers indicate a cache which is idle with respect to writes.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### cachedBytes
The total number of bytes that have been written into snapshots.
This statistic is updated during the creation of a snapshot.
The purpose of this statistic is to allow calculation of cache throughput between any two instances of time.
The ratio of the difference between two samples of this statistic divided by the
interval separating the samples is a measure of the cache throughput (more accurately,
the rate at which data is being snapshotted). When combined with the `diskBytes`
and `memBytes` statistics, it can also be used to calculate the rate at which data
is entering the cache and rate at which is being purged from the cache.
If the entry rate exceeds the exit rate for a sustained period of time,
there is an issue that needs to be addressed.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### diskBytes
The size, in bytes, of on-disk snapshots.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### memBytes
The size, in bytes, of in-memory cache.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### snapshotCount
The current level (number) of active snapshots.
In a healthy system, this number should be between 0 and 1. A system experiencing
transient write errors might expect to see this number rise.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### WALCompactionTimeMs
The duration, in milliseconds, that the commit lock is held while compacting snapshots.
The expression `(cachedBytes - diskBytes) / WALCompactionTime` provides an indication
of how fast the WAL logs are being committed to TSM files.
The ratio of the difference between the start and end "WALCompactionTime" values
for an interval divided by the length of the interval provides an indication of
how much of maximum cache throughput is being consumed.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeDropped
The total number of writes dropped due to timeouts.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeErr
The total number of writes that failed.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeOk
The total number of successful writes.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### tsm1_engine
The `tsm1_engine` measurement statistics are related to the usage of a TSM storage
engine with compressed blocks.

#### cacheCompactionDuration  
The duration (wall time), in nanoseconds, spent in cache compactions.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### cacheCompactionErr
The number of cache compactions that have failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### cacheCompactions  
The total number of cache compactions that have ever run.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### cacheCompactionsActive
The number of cache compactions that are currently running.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmFullCompactionDuration
The duration (wall time), in nanoseconds, spent in full compactions.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmFullCompactionErr
The total number of TSM full compactions that have failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmFullCompactionQueue
The current number of pending TMS Full compactions.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmFullCompactions
The total number of TSM full compactions that have ever run.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmFullCompactionsActive
The number of TSM full compactions currently running.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmLevel1CompactionDuration
The duration (wall time), in nanoseconds, spent in TSM level 1 compactions.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel1CompactionErr
The total number of TSM level 1 compactions that have failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel1CompactionQueue
The current number of pending TSM level 1 compactions.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmLevel1Compactions
The total number of TSM level 1 compactions that have ever run.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel1CompactionsActive
The number of TSM level 1 compactions that are currently running.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmLevel2CompactionDuration
The duration (wall time), in nanoseconds, spent in TSM level 2 compactions.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel2CompactionErr
The number of TSM level 2 compactions that have failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel2CompactionQueue
The current number of pending TSM level 2 compactions.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmLevel2Compactions
The total number of TSM level 2 compactions that have ever run.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel2CompactionsActive  
The number of TSM level 2 compactions that are currently running.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmLevel3CompactionDuration  
The duration (wall time), in nanoseconds, spent in TSM level 3 compactions.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel3CompactionErr
The number of TSM level 3 compactions that have failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel3CompactionQueue  
The current number of pending TSM level 3 compactions.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmLevel3Compactions
The total number of TSM level 3 compactions that have ever run.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmLevel3CompactionsActive
The number of TSM level 3 compactions that are currently running.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmOptimizeCompactionDuration
The duration (wall time), in nanoseconds, spent during TSM optimize compactions.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmOptimizeCompactionErr
The total number of TSM optimize compactions that have failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmOptimizeCompactionQueue
The current number of pending TSM optimize compactions.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### tsmOptimizeCompactions
The total number of TSM optimize compactions that have ever run.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### tsmOptimizeCompactionsActive
The number of TSM optimize compactions that are currently running.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

---

### tsm1_filestore
The `tsm1_filestore` measurement statistics are related to the usage of the TSM file store.

#### diskBytes
The size, in bytes, of disk usage by the TSM file store.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### numFiles
The total number of files in the TSM file store.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

---

### tsm1_wal
The `tsm1_wal` measurement statistics are related to the usage of the TSM Write Ahead Log (WAL).

#### currentSegmentDiskBytes
The current size, in bytes, of the segment disk.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### oldSegmentDiskBytes
The size, in bytes, of the segment disk.

**Data type:** Integer  
**Metric type:** [Gauge](#gauge)

#### writeErr
The number of writes that failed due to errors.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeOK
The number of writes that succeeded.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

---

### write
The `write` measurement statistics are about writes to the data node, regardless of the source of the write.

#### pointReq
The total number of every point requested to be written to this data node.
Incoming writes have to make it through a couple of checks before reaching this
point (points parse correctly, correct authentication provided, etc.).
After these checks, this statistic should be incremented regardless of source
(HTTP, UDP, `_internal` stats, OpenTSDB plugin, etc.).

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointReqHH (Enterprise only)
The total number of points received for write by this node and then enqueued into
hinted handoff for the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointReqLocal (Enterprise only)
The total number of point requests that have been attempted to be written into a
shard on the same (local) node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### pointReqRemote (Enterprise only)
The total number of points received for write by this node but needed to be forwarded into a shard on a remote node.
The `pointReqRemote` statistic is incremented immediately before the remote write attempt,
which only happens if HH doesn't exist for that node.
Then if the write attempt fails, we check again if HH exists, and if so, add the point to HH instead.  
This statistic does not distinguish between requests that are directly written to
the destination node versus enqueued into the hinted handoff queue for the destination node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### req
The total number of batches of points requested to be written to this node.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### subWriteDrop
The total number of batches of points that failed to be sent to the subscription dispatcher.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### subWriteOk
The total number of batches of points that were successfully sent to the subscription dispatcher.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeDrop
The total number of write requests for points that have been dropped due to timestamps
not matching any existing retention policies.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeError
The total number of batches of points that were not successfully written,
due to a failure to write to a local or remote shard.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeOk
The total number of batches of points written at the requested consistency level.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writePartial (Enterprise only)
The total number of batches of points written to at least one node but did not meet
the requested consistency level.

**Data type:** Integer  
**Metric type:** [Counter](#counter)

#### writeTimeout
The total number of write requests that failed to complete within the default write timeout duration.
This could indicate severely reduced or contentious disk I/O or a congested network to a remote node.
For a single write request that comes in over HTTP or another input method, `writeTimeout`
will be incremented by 1 if the entire batch is not written within the timeout period,
regardless of whether the points within the batch can be written locally or remotely.

**Data type:** Integer  
**Metric type:** [Counter](#counter)