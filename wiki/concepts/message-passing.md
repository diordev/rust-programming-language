---
title: "Message Passing"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, concurrency, threads, channels, patterns]
source_count: 2
---

# Message Passing

## Short Definition

Concurrency modeli: threadlar shared memory orqali emas, **xabar uzatib** muloqot qiladi.

## Why It Matters

Shared mutable state ko'p concurrency xatolarining manbai. Message passing bu xatolarni oldini olishning samarali usullaridan biri.

## Mental Model

Threadlar — alohida ofislar. Bir-biri bilan post orqali xat yozishadi. Xat yuborilgandan keyin ownership o'tadi.

Bu modelda queue semantics ham muhim: unbounded channel producer'ni deyarli ushlamaydi, bounded [[sync-channel]] esa consumer ortda qolsa producer'ni block qilib backpressure beradi.

## Syntax and Examples

```rust
use std::sync::mpsc;

let (tx, rx) = mpsc::channel();
std::thread::spawn(move || {
    tx.send(String::from("hello")).unwrap();
});
println!("Got: {}", rx.recv().unwrap());
```

## Message Passing vs Shared State

| Pattern | Mexanizm |
|---------|----------|
| Message passing | `mpsc::channel` |
| Shared state | `Mutex<T>`, `Arc<T>` |

## Common Mistakes

- **Message passing'da ham deadlock bo'ladi deb o'ylamaslik**.
- **Shared state pattern'ga "yomon" deb qarash**.
- **Shutdown semanticsni unutish** — sender close ko'pincha lifecycle signal bo'ladi.

## Related Concepts

- [[channels]]
- [[concurrency]]
- [[threads]]
- [[ownership]]
- [[mutex-t]]
- [[sync-channel]]

## Sources

- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
