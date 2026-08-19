# Lec 9
---

Both papers leverage the fact that NFS(till v3) is a stateless protocol.

2 traditional ways of catchup of backup of replicas:
- Send snapshots/checkpoints
- Send a replay log of all the inputs since the backup went down.

---

HA-NFS -> NFSv2.
Very assumption => stateless protocol

---

Transaction is complete when the transaction end +xe is flushed to the journalling part of the disk.

Journal is before the in-place region
in-place region => inodes + data + bitmap etc
Journl + in-place = disk

async writes
metadata-intensive => small file ops => create/del etc

durable => persistent

---



---
