---
title: "per-request thread::spawn"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, threads, concurrency, web-server]
source_count: 1
---

# per-request thread::spawn

Listing 21-11 har incoming stream uchun alohida thread yaratadi.

```rust
for stream in listener.incoming() {
    let stream = stream.unwrap();

    thread::spawn(move || {
        handle_connection(stream);
    });
}
```

## Why It Matters

Bu single-threaded bottleneck'ni tezda yo'qotadi. Lekin har request uchun yangi thread spawn qilish cheksiz resource growth beradi, shuning uchun bu transition bosqichi, final yechim emas.

## Related

- [[threads]]
- [[thread-pool]]
- [[send-trait|Send trait]]
- [[static-lifetime|static lifetime]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
