# Lec 11 - Chain replication
---

P/B:
Reads must wait for writes to complete for strong consistency.
Read do not need to wait for writes to complete for weak consistency.
Remember that for a write to complete in P/B, the writes must be propogated to all the backups.

---

Chain replication works only well with LANs, splitting the chains across datacenters would not be
good for performance.
CRAQ solves this by allowing any node in the chain to reply to queries/reads -- any node in any
datacenter can reply to the client.
Writes still suffer though, there's no way around it, this is a system designed for mainly read
heavy workloads.

---

What to do on loss?
Client is responsible for remembering that an update has failed and should do some recovery steps to
ensure that the operation has been performed on the chain replication.
If the client gets the ack, we can be confident that the chain remembers the update.

---

If the middle node fails there will be some cleanups required for the ack part as well. Not really
discussed in the paper but yea would be fun to implement lolol.

---


