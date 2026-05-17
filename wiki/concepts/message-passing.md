---
title: "Message Passing"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, channels, patterns]
source_count: 1
---

# Message Passing

## Short Definition

Concurrency modeli: threadlar shared memory orqali emas, **xabar uzatib** muloqot qiladi. Go til hujjatidan slogan: *"Do not communicate by sharing memory; instead, share memory by communicating."*

## Why It Matters

Shared mutable state ko'p concurrency xatolarining manbai. Message passing bu xatolarni oldini olishning eng samarali usullaridan biri:
- Yuborilgan qiymat ownership o'tkaziladi → asl thread endi unga kira olmaydi.
- Receiver ham bir vaqtda faqat bitta qiymatni qabul qiladi.
- Race conditionlar tabiiy ravishda yo'q.

Rustda message-passing [[channels]] orqali amalga oshiriladi va ownership system avtomatik ravishda xavfsizlikni kafolatlaydi.

## Mental Model

Threadlar — alohida ofislar. Bir-biri bilan post orqali xat yozishadi. Hech qaysi ofis ikkinchisining ish stoliga aralashmaydi. Xat yuborilgandan keyin nusxa qolmaydi (Rustda) — original ofisda hujjat yo'q.

## Syntax and Examples

```rust
use std::sync::mpsc;
use std::thread;

let (tx, rx) = mpsc::channel();

thread::spawn(move || {
    tx.send(String::from("hello")).unwrap();
});

println!("Got: {}", rx.recv().unwrap());
```

Batafsil: [[channels]].

## Message Passing vs Shared State

| Pattern | Mexanizm | Sinxronizatsiya |
|---------|----------|------------------|
| Message passing | `mpsc::channel` | Channel ichidagi `Send` qoidalari |
| Shared state | `Mutex<T>`, `Arc<T>` | Lock olishni boshqarish |

Ikkalasi ham Rustda mavjud — vaziyatga qarab tanlanadi. Ko'pincha message passing aniqroq va xavfsiz, lekin ba'zan shared state samaraliroq.

## Common Mistakes

- **Message passing'da ham deadlock bo'ladi deb o'ylamaslik** — recvers va senders bir-birini kutib qolishi mumkin.
- **Shared state pattern'ga "yomon" deb qarash** — vaziyatga qarab har biri to'g'ri tanlov.

## Related Concepts

- [[channels]]
- [[concurrency]]
- [[threads]]
- [[ownership]]
- [[mutex-t]]

## Sources

- [[16-2-transfer-data-between-threads-with-message-passing]]
