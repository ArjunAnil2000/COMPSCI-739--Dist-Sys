# Lecture 3 -- Failures and Redundancy
---

 Paper crashes summary

 4.1 System Behavior Analysis:
    ◦ 4.1.1 Redis: Does not use checksums for user data; corruption on a leader can be propagated to followers during re-synchronization.
    ◦ 4.1.2 ZooKeeper: While it uses checksums, it often crashes. A bug in error handling causes a partial crash (only some threads die), leading to indefinite write unavailability because a new leader is never elected.
    ◦ 4.1.3 Cassandra: Checksums are only verified if compression is enabled. Without it, a read-repair protocol might propagate a corrupted value if it is lexically greater than the original.
    ◦ 4.1.4 Kafka: Often crashes on errors. Corruption in the leader can cause followers to hit a fatal assertion and crash, leading to cluster unavailability.
    ◦ 4.1.5 RethinkDB: Lacks checksums for data blocks and relies on the file system for integrity, leading to silent data loss if a metablock or transaction head is corrupted.
    ◦ 4.1.6 MongoDB: Generally crashes on errors but can sometimes recover from journal corruption by contacting the leader.
    ◦ 4.1.7 LogCabin: Uses the Raft protocol and can fix corrupted logs by fetching data from a new leader, though it crashes on errors in closed segments.
    ◦ 4.1.8 CockroachDB: Primarily crashes on faults; metadata faults can lead to silent data loss where the system returns zero rows.

• 4.2 Observations across Systems:
    ◦ #1: Diverse Strategies: Systems range from total trust in lower layers (Redis) to careful checksumming (ZooKeeper).
    ◦ #2: Local Behavior: Faults are often undetected; when they are detected, crashing is the most common reaction, which reduces cluster redundancy.
    ◦ #3: Redundancy is Underutilized: Single faults cause cluster-wide disasters because systems rarely look to other replicas to fix a local storage fault. Small faults can affect an inordinate amount of data (e.g., losing an entire log).
    ◦ #4: Crash and Corruption Handling are Entangled: Systems often use the same code for both. For instance, Kafka might truncate a log (losing data) because it misinterprets corruption as a signal of a system crash.
    ◦ #5: Protocol Nuances Spread Corruption: Subtle implementation details in leader election or read-repair can spread corrupted data to healthy nodes.

---

| Failure | Def |
| --- | --- |
| Halting | 4 | 
| Fail-stop | 2 | 
| Omission | 3 | 
| Network | 7 | 
| Network partition | 6 | 
| Timing | 1 | 
| Byzantine | 5 | 

---

Fail-stop: Components let the other nodes know that it has failed so that the other components can
perform any recovery ops accordingly.

---

disk-to-disk sorting benchmark(old stuff)

---


