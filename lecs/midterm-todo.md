# Midterm prep
---

## Lec 2 RPC

- Learn how the RPC semantics for transfer works. Re-read the paper

## Lec 3 Failures + Redundancy

- Remember the techniques from the Failures paper
- Revisit Redundancy

## Lec 4 NFS

- what state does the client store in NFS?
    - file handles
    - caches, as required
    - what else?

- NFS v1
    - adv and disadv
    - what state does the client store
- NFS v2
    - what caches did the implementation stuff use?
        - read/write cache on the client, for r/w performance in general. Writes were flush on
          close() with the write-through policy (close blocks till the write changed bytes end up in
          the server's durable storage).
        - attr cache on the client for freq getattr requests.
        - nvram write buffer on server.
- NFS v3
    - Does the async write get buffer on the server's memory till a commit arrives or is it flushed
      in the background?
      => I think it's flushed in the background gradually.
    - 
- NFS v4
    - The only RPCs calls, in the strictest sense, are the NULL and the COMPOUND procedures and
      their callback analogues.
    - ***READ THIS AGAIN***
    - TCP is mandatory
    - Delegations vs locks -- important
