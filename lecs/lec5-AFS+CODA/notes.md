# Lec-5 AFS + CODA
---

## Questions: AFS
Problem with AFS not being used for a distributed database?
- High concurrency write is required for most distributed databases.
Can we not implement a distributed database in AFS by using a very small file and the lock-server?
Read => good, Writes => lock would take care of it but be slow.

After a client reboots and the data cache (persistent) is marked suspect, the status cache is used
to check if the file is still valid or needs to be re-fetched. However, the status cache is on
volatile memory for performance reasons. Q: Is the status cache rebuilt from the data cache (main
thing it requires is the modification time ig) after a client reboots?

## Questions: CODA
A small change in the file would require the entire file to be sent across. Isn't this highly
inefficient?

---

Consistency model for AFS is good => clear semantics since the same file consistently from open() to
close(). If there is an another client writing to the file, the consistency of the local file in the
current replica is not affected by it. Only during a close() will the consistency come into question
and conflic resolution happens. This is a double-edged sword when writes() from different clients to
the same file.

---

Drawbacks on whole file caching on ***AFS***?
- Different semantic as compared to local.
    - Locally if different clients (processes) were r/w to a file, there's a shared page cache,
      changes are reflected immediately on r/w.
    - AFS, the changes are kept to one client until a close() is called. And even after that, it
      needs to be careful to not pull a file which it has already locally written.

---

> [!IMPORTANT]
> AFS' callback breaking is only checked when a file is open()ed. Not immediately when the callback
> is broken by the server.
> This means that a callback can be broken at time 100 and the client can continue to read/write
> from the local copy until the next open() happens (obviously after a close() happens).

---

# CODA
---

CAP theorom
The CAP theorem, or Brewer's Theorem, states that a distributed database system can only provide two
of three guarantees: Consistency (all nodes see same data), Availability (every request gets a
response), and Partition Tolerance (system works despite network failures). During a network
partition, a system must choose between consistency and availability. 

CODA chooses availability during a network parition => opimistic replica control is preferred over
pessimistic replica control.

---

(If lease was used on file in the local copy on CODA)
When lease expires?
- No more access to the local copy of the file (availability :down:)
- Local copy of the work would need to be thrown away.

---

Optimical replica control is not good for consistency critical workloads like financial stuff.

---

When does file fetch()ing happen? 
AFS -- only open()
CODA refetches file at open(), hoard walks also fetches -- slightly worse performance because of it.

---

Storeid is used every time a STORE is called. This is used to detect various conflicts like
write-write conflicts.
