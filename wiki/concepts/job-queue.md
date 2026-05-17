---
title: "Job Queue"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, concurrency, queue, channels]
source_count: 1
---

# Job Queue

## Short Definition

Thread pool'ga kelgan ishlar vaqtincha saqlanadigan navbat. Chapter 21.2'da bu queue `mpsc::channel()` orqali model qilinadi va `Job = Box<dyn FnOnce() + Send + 'static>` qiymatlarini tashiydi.

## Why It Matters

Queue bo'lmasa pool faqat "darhol bo'sh worker topilsa ishlaydi" modelida qoladi. Queue workerlar band paytda keyingi request'larni yo'qotmasdan kutishga imkon beradi va backpressure yaratadi.

## Mental Model

`execute` - producer.
Workerlar - consumer.
Queue - ular orasidagi tampon.

Std channel `mpsc` bo'lgani uchun producer ko'p bo'lishi mumkin, consumer esa bitta receiver orqali boshqariladi.

## Syntax and Examples

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;

let (sender, receiver) = mpsc::channel::<Job>();
let receiver = Arc::new(Mutex::new(receiver));

self.sender.send(Box::new(f)).unwrap();
```

## Common Mistakes

- **Queue yo'q deb o'ylash**: channel aynan queue vazifasini bajaradi.
- **Receiver'ni har workerga ownership bilan berish**: bu `E0382` va `mpsc` single-consumer qoidasi bilan to'qnashadi.
- **Queue lock'ini task execution davomida ushlab turish**: throughput pasayadi.

## Related Concepts

- [[channels]]
- [[thread-pool]]
- [[worker-thread]]
- [[type-alias]]
- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]

## Sources

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
