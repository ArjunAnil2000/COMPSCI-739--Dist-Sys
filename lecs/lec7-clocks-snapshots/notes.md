# Lec - 7 - Ordering and Snapshots
---

RSM -> Replicated State Machine

---

If the central time server for physical clocks is behind and asks a client to set the time back,
this is a problem since setting a clock back is always a bad idea.
Spanner -> true time -> physical clocks => later in the semester.

---

Concurrency is not transitive.

---

Vector clocks is a process-wise clock where each process in the vector is a partially ordered
logical clock.

---


