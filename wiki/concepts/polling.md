---
title: "Polling"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, futures]
source_count: 1
---

# Polling

## Short Definition

Polling — runtime'ning future tayyor yoki yo'qligini tekshirish jarayoni. `Future::poll` metodi chaqirilganda `Poll::Ready(value)` yoki `Poll::Pending` qaytaradi.

## Why It Matters

Polling Rust async modelining asosiy mexanizmi. Future'lar o'z-o'zidan "tayyor bo'ldim" deb xabar bermaydi — runtime ularni tekshirib turadi. Bu **pull-based** model: runtime so'raydi, future javob beradi.

## Mental Model

Polling'ni restoran so'rash jarayoni sifatida tasavvur qiling:
- Ofitsiant (runtime) har stolja (future) yaqiniga kelib so'raydi: "Tayyor?"
- Tayyor bo'lsa ovqat beriladi (`Ready`)
- Yo'q bo'lsa: "Bir ozdan keyin qaytaman" (`Pending`)

Zamonaviy runtime'lar aslida **waker** mexanizmi orqali ishlaydi: future `Pending` qaytarganda, tayyor bo'lganda runtime'ni xabardor qilish uchun `Waker` ob'ektini saqlab qo'yadi. Bu runtime'ni bekor polling'dan saqlaydi.

## Syntax and Examples

`Future` trait (to'liq):

```rust
pub trait Future {
    type Output;

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}

pub enum Poll<T> {
    Ready(T),  // tayyor, qiymat mavjud
    Pending,   // hali tayyor emas
}
```

Oddiy Future implementatsiyasi:

```rust
use std::future::Future;
use std::pin::Pin;
use std::task::{Context, Poll};

struct ImmediateFuture {
    value: i32,
}

impl Future for ImmediateFuture {
    type Output = i32;

    fn poll(self: Pin<&mut Self>, _cx: &mut Context<'_>) -> Poll<i32> {
        Poll::Ready(self.value) // har doim darhol tayyor
    }
}
```

Async kodni yozganda polling'ni to'g'ridan-to'g'ri chaqirmasiz — compilator va runtime buni boshqaradi:

```rust
async fn example() {
    let result = some_future.await; // runtime poll qiladi
}
```

## Common Mistakes

- **Polling'ni to'g'ridan-to'g'ri chaqirish**: Oddiy async kod yozayotganda `poll` chaqirilmaydi — faqat custom `Future` implementatsiyasida kerak.
- **Waker tushunchasini o'tkazib yuborish**: Samarali future `Pending` qaytarganda waker'ni saqlashi kerak, aks holda runtime uni hech qachon qayta tekshirmaydi.

## Related Concepts

- [[future|Future trait]] — poll metodi shu traitda
- [[async-runtime|async runtime]] — polling'ni amalga oshiruvchi
- [[async-state-machine|async state machine]] — har bir poll call state'ni o'zgartirishi mumkin
- [[async-await|async/await]] — polling'ning abstraksiyasi

## Sources

- [[17-1-futures-and-the-async-syntax]]
