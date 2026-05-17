---
title: "slow /sleep request"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, web-server, concurrency, http]
source_count: 1
---

# slow /sleep request

Listing 21-10 `GET /sleep HTTP/1.1` case'ini qo'shib, single-threaded server bottleneck'ini ko'rsatadi.

```rust
let (status_line, filename) = match &request_line[..] {
    "GET / HTTP/1.1" => ("HTTP/1.1 200 OK", "hello.html"),
    "GET /sleep HTTP/1.1" => {
        thread::sleep(Duration::from_secs(5));
        ("HTTP/1.1 200 OK", "hello.html")
    }
    _ => ("HTTP/1.1 404 NOT FOUND", "404.html"),
};
```

## Why It Matters

Bu kod serverni "yomon" qilish uchun emas, muammoni ko'rinadigan qilish uchun yoziladi. Bitta sekin request orqasida boshqa request'lar ham turib qoladi.

## Related

- [[web-server]]
- [[threads]]
- [[thread-pool]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
