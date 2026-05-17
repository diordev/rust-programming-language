---
title: "TcpListener connection loop"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, tcp, web-server]
source_count: 1
---

# TcpListener connection loop

Listing 21-1 serverning eng sodda skeletini ko'rsatadi: socket'ga bind bo'lish va incoming connection'larni qabul qilish.

```rust
use std::net::TcpListener;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        println!("Connection established!");
    }
}
```

## Why It Matters

Bu yerda hali HTTP yo'q. Lekin web serverning birinchi haqiqiy qadami shu: OS darajasida port ochib, browserdan kelgan ulanishni qabul qilish.

## Related

- [[tcp-listener]]
- [[tcp-stream]]
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|Chapter 21.1]]
