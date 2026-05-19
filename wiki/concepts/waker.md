---
title: "Waker"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, futures]
source_count: 1
---

# Waker

## Short Definition

`Waker` - [[future|Future]] tayyor bo'lganda [[executor]]ga "meni yana `poll` qil" deb signal beradigan obyekt.

## Why It Matters

`Poll::Pending` qaytargan future keyin qachon tayyor bo'lishini executor o'zi bilmaydi. `Waker` shu aloqani yopadi: future yoki uning orqasidagi I/O/timer mexanizmi tayyorlik paytida executor'ni uyg'otadi.

## Mental Model

Executor future'ga telefon raqamini qoldiradi. Future hali tayyor bo'lmasa, shu raqamni saqlaydi. Keyin ish tugaganda qo'ng'iroq qiladi. Shu qo'ng'iroq `Waker::wake()`dir.

## Syntax and Examples

`poll` ichida waker olinadi:

```rust
fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output> {
    let waker = cx.waker().clone();
    // keyinroq tayyor bo'lganda:
    waker.wake();
    Poll::Pending
}
```

Minimal executor waker'i taskni yana queue'ga yuborishi mumkin:

```rust
impl Wake for TaskWaker {
    fn wake(self: Arc<Self>) {
        let _ = self.sender.send(self.task.clone());
    }
}
```

## Common Mistakes

- `Pending` qaytarib, waker'ni saqlamaslik. Bunday future qayta `poll` qilinmasligi mumkin.
- `wake` natijani uzatadi deb o'ylash. `wake` faqat executor'ga signal beradi; natija keyingi `poll`da olinadi.
- Har `poll`da yangi background ishni takror boshlash. Custom future ichida tayyorlik holati saqlanishi kerak.

## Related Concepts

- [[task-context|Context]]
- [[poll-enum|Poll]]
- [[polling]]
- [[executor]]
- [[future|Future]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

