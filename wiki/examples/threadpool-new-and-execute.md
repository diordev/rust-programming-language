---
title: "ThreadPool::new and pool.execute"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, thread-pool, api-design, compiler]
source_count: 1
---

# ThreadPool::new and pool.execute

Listing 21-12 desired public API'ni oldin yozadi va keyin implementation compiler yordamida quriladi.

```rust
let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
let pool = ThreadPool::new(4);

for stream in listener.incoming() {
    let stream = stream.unwrap();

    pool.execute(|| {
        handle_connection(stream);
    });
}
```

## Why It Matters

Bu snippet hali boshida compile bo'lmaydi. Lekin aynan shu desired usage keyingi design'ni yo'naltiradi: `ThreadPool`, `new`, `execute`, `FnOnce + Send + 'static`, va queue-based workers.

## Related

- [[thread-pool]]
- [[compiler-driven-development]]
- [[fn-traits|Fn traits]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
