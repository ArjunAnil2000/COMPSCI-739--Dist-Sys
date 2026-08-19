# Lec-4
---

NFSv2 requires the clients to send completely self-contained requests from the client to the server.
What does this mean?

---

How does append work in NFSv2?

---

How did NFSv2 handle non-idempotent ops?

- Reply-cache like in RPCs where we can remember many past RPCs that need to be remembered. The
  return code and the RPC id need to be cached. If the same id is seen, return code.

What if server crashes?
- Server lose reply cache, return confusing result.
- Server remembers nothing!

---

Disadvantages for NFSv2 stateless server

- Writes.
    - Usually keeped buffered in client.
    - If sent immediately, the server (write-through policy) means horrible performance.
    - You're allowed to send reads before that but it's not required.
    - Compromise: Send some buffers in the background but do not block the application on this.
    - In most cases, write() is kept buffered until a close() is seen where all the dirty buffers
      are flushed for the particular file. 
        - The close() has to make the write persistent on the server disk.
=> This means that the closes() are going to be very slow.
- This is solved by keeping a non-volatile memory based buffer on the server. Expenses up but
  still performance is better.

---

NFSv3 client caching: consistency

- Multiple clients read/write to the same file.

Client cache is required for performance but what's the fundamental problem with this?
- Don't know about the shared state i.e A client is writing to the file while B is reading from it.

How can client discover data is stale?
- Poll using getattr() to get the mtime and validate the cache. If mtime is different, invalidate.
- But this means a lot of getattr() requests. So the result is cached for a certain amount of time
  and then invalidated => implementation details. Might be put in the servers.

performance probs:
- Lot of getattr()s which is a big problem => cache the getattr result for sometime.

---

Distributed locks 

Can stateless server provide locks for files?

Solution?
in NFSv2?
- part of another service
- Not our problem lol

---

NFSv3 addresses 3 proble,s:
1. 64-bit file size and offset support
2. Perf problems with write()
3. Too many getattr()s

---

NFSv3: Improves write performance

- write-commit works on the verifier. If the verifier has changed it means that the server has
  crashed in between and the client probably needs to replay all the writes.
- verifier is usually boot-time and violates the stateless server policy (which is fine).

---

NFSv3: Uncommitted Data can be lost.

Should a client read the old data or the new data?
old => on-disk
new => un-commited data
- read only committed data because a crash will lose all the uncommitted data. Therefore, if a
  uncommitted data is returned and a server crash happens and the read is returned, new data is read
  first and then after sometime old data is returned. Therefore, it's better to return old.
These are implementation details.

v2 suffered from too many getattrs(). How does v3 solve this?
- any call returns attr - per file.
- all files in dir

---

NVSv4: Introduces open() + close()

When can future opens()
- Read about this and delegations in detail

---

NFSv4
If the server crashes does it always wait lease_interval() time before serving?

---


