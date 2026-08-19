# Lec-6-2026-02-11
---

## Leases

### Leases-physical clocks
- It's better for the client clock to run faster since the other way around, the client will think
  it has the lease when the server's clock has progressed far enough that it thinks that particular
  client's lease has already expired.
- The physical time of the client and the server does not matter. It's the delta that matters.
- Communication time << lease time, otherwise it would need to be included in the calculations.

### Lease:Term Length

Short lease adv:
- Recovery speed is very fast -- minimizes delay after failure. A crash client's lease will auto
  expire in sometime on the server. This is for exclusive leases.
- Minimize false-sharing.
- Reduces the storage load on the server -- it needs to track all the granted leases and a shorter lease
  means that a state is held for a shorter time 

Long lease adv:
- Improve perf -- more efficient while clients are accessing.
- They don't have to ask for extensions -- less network load due to leases.
- Works best when there's little sharing going on -- the file does not get changed a lot.

> [!NOTE]
> False sharing: 2 clients are sharing a file but they do not know that the other client is sharing
> the file. This results in inconsistent files if both clients are writing at the same time.

> [!IMPORTANT]
> Keep CAP theorem in mind when talking about all this.
> C-Consistency, A-Availability, P-Partition Tolerance.
> Cannot have all 3, sacrifice one for the rest.

Mutli-cast when server for installed files:
- Assume that the server always notifies the client about the lease for these installed files. Or in
  other words the client don't ask for a lease extension but wait for a server to sent a multi-cast.

---

## lbfs => Low Bandwidth File System

What is the file is modified outside of LBFS?
Recheck that the file A actually matches by computing SHA1 again.
The chunk database is thus a collection of hints and the SHA1 hash is calculated for every chunk by
the server and the client.

If everything changes, all the write data needs to be sent anyway and the CONDWRITE calls were a
useless round trip.

Small files -> rountrips

---

