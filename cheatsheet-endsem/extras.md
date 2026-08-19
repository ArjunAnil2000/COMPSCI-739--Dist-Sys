# GFS
- relaxed/weak consistency.
- high sustained bandwidth is more important than latency. 
- client and chunkservers do not cache data. metadata is cached instead.
- metadata kept in memory of master -- a) namespaces; b) file-to-chunk mapping --. c) chunk to chunkserver mapping. a), b) are persisted while c) is not.
- locking a path requires read locks on parents and write lock only one the file modified. Directories do not have metadata.
- deletion happens by renaming to special name. master deletes that after a fixed interval. heartbeat messages reply from master state identity of chunks that are no longer present in the master's metadata. deletion can happen after this.
- heartbeat messages are from the chunkserver to the master.
- chunk version number for detecting stale replicas.
- shadow masters which are read-only.
- checksumming to detect corruptions/inconsistencies.

# Dynamo
- eventually consistent data store.
- partitioning--consistent hashing--incremental scalability.
- high availability for writes--vector clocks with reconciliation during reads
- handling temp failures--sloppy quoram with hinted handoff
- handling perm failures--anti-entropy using merkle trees
- membership and failure detection--gossip based membership protocol and failure detection.
- syntactic and semantic reconciliation for verctor clocks.
- get and put return a context which has version vectors. The version vector has (node, version counter) for all the nodes to which a write for this particular object has gone fo

# Big Table
- eventual consistency by the looks if it (not sure).

# Spanner
- strong consistency (externally consistent -- linearizable). globally consistent read @ timestamp.
