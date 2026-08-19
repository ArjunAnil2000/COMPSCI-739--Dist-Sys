# Midterm sample papers learnings/todo
---

## Generic

- Check which FS provides network partition support. Only CODA does AFAIK.
- Ordering and consistency. Checkout which paper has this and throuroughly understand the
  requirements.

## Failures

- Go through availability and reliability definitions and the equation for MTTR+MTBF again. Ensure
  to understand what is proportionate to what.

## RPCs

- Read the table part again
- Read the entire paper again, or the slides.
- RPCs cannot be used with UNIX semantics for read/writes. This is because read/write with an fd
  would implicitly move the fd pointer causing the operation to be non-idempotent. The correct way
  to implement something like this would be to do something like a pwrite and pass along the
  particular offset that the operation must be peformed. 

Some question-answers:

- The RPC-i Server (callee machine) benefits from tracking which clients (callers) have imported the
  related service
  False; RPC servers do not track which clients have imported the service; that would make the state
  on the server not scale with more clients
- The RPC-i Server (callee machine) does not need to track call id numbers from each client (caller)
  True; with regular RPC, the server tracks call id numbers to discard duplicates in order to make
  sure it doesn’t re-execute (non-idempotent) operations; with i-RPC, it is okay to re-execute so we
  don’t need to track if the server has seen this call before

- More more than one-outstanding-request based RPC design having an idem-potent operations only
  requirement helps in making the implementation easier but is **not** a required per-se. In fact,
  one can imagine that in an idem-potent only operations design RPC, keeping the last serviced
  request id is not important since the requests do not change the state anyway => requests can be
  serviced in any order possible.

## NFS

- Write down which feature was offered in which version of NFS.
- Read more about async writes for NFSv3 in detail.
- NFSv2 uses a reply cache for the non-idempotent (mkdir) operation in case of duplicates.

Some question-answers:

- All use RPCs.
- Only NFSv3 can lose data that was written to the server -- async-writes.
- 2 concurrent client writing to the same file can be intermixed on the server -- no version gives
  implicit locking. Even NFSv4 provides mechanisms for locking, in build but does not turn it on by
  default.
- All can keep writes buffered locally until close(). However, only NFSv4 can keep it buffered
  locally after close.

## AFS + CODA

- Read the reintegration logic of CODA again.
- What is transferred in AFS as part of close()? Whole file? Delta writes? Implementation detail,
  but for the exam, assume that either is possible and answer.

Some question-answers:

- When a write-write conflict happens during reintegration in CODA, it is not possible for CODA to
  guarantee to have the same file contents as AFS in the same scenario.

## Lamport clocks

- Read the question carefully. If it says to use only the logical clock timestamps, do NOT use the
  graph for ordering.
- Logical clocks -> cannot tell `before` or `concurrent` relationship based on just the clovk values
  of the different processes. Vector clocks -> *can* tell the `before` or `concurrent` relationship
  based on just the clock values.
- Logical clocks -> can set initial value to anything. By communicating messages, the clocks of all
  the nodes gradually get synced to satisfy the clock condition.
  Vector clocks -> cannot set initial value to anything since it would not be possible to infer
  "happens before" based on a random init value.

## Distributed snapshot

- Read about the stable property part, go through the slides once again.

## HA-NFS

- Go through the AIX journaling part clearly and the other parts surrounding that. Should be there
  in the ppt.

## Chain replication

- Checkout the adding new node to tail part.
- Checkout what happens when a node fails (head/tail/intermediate node).
