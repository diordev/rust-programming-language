---
title: "incoming().take(2) graceful shutdown demo"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, iterator, shutdown, web-server]
source_count: 1
---

# incoming().take(2) graceful shutdown demo

21.3 graceful shutdown code ishlayotganini ko'rsatish uchun server faqat 2 ta request qabul qiladi.

```rust
let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
let pool = ThreadPool::new(4);

for stream in listener.incoming().take(2) {
    let stream = stream.unwrap();

    pool.execute(|| {
        handle_connection(stream);
    });
}

println!("Shutting down.");
```

## Why It Matters

Bu production behavior emas. Maqsad: loop tugagach pool scope'dan chiqishini va `Drop` avtomatik ishga tushib, sender'ni yopib, workerlarni join qilayotganini ko'rsatish.

## Related

- [[graceful-shutdown]]
- [[thread-pool]]
- [[worker-thread]]
- [[web-server]]
