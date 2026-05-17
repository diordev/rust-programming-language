---
title: "17.5. A Closer Look at the Traits for Async"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, traits, pin]
source_count: 1
---

# 17.5. A Closer Look at the Traits for Async

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-05-traits-for-async.html
- Raw: `raw/books/the_rust_programming_language/17.5. A Closer Look at the Traits for Async.md`

## Detailed Summary

Bu bo'lim `Future`, `Pin`, `Unpin`, va `Stream` trait'larining ichki mexanizmlarini ochib beradi. Kundalik async kodi uchun bu chuqurlik shart emas, lekin `join_all` kabi joylarda xatoliklar tushganida yordam beradi.

### Future trait to'liq ta'rifi

```rust
pub trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}

pub enum Poll<T> {
    Ready(T),
    Pending,
}
```

`poll` metodining ikkita g'ayrioddiy jihati:
1. `self: Pin<&mut Self>` — oddiy `&mut self` emas, balki `Pin`-wrapped reference
2. `cx: &mut Context<'_>` — runtime'ga "tayyor bo'lganda menga xabar ber" deyish vositasi (Waker)

`await` aslida `poll`ni takroriy chaqiruvchi loopdir: `Ready(value)` qaytarguncha. Lekin to'g'ridan-to'g'ri blocking loop emas — runtime boshqaradi.

### Self-referential future va Pin muammosi

Async blok state machine'ga aylantirilganda, ba'zi variantlar **o'z-o'ziga reference** saqlashi mumkin:

```
PageTitleFuture {
    GetAwaitPoint { url: &str }  ← o'zidagi String'ga reference
}
```

Bunday tur ko'chirilsa (moved), eski manzilga ko'rsatuvchi reference invalid bo'lib qoladi — **dangling reference** yaratiladi. Bu xavfli.

`Pin<Box<T>>` — `T`ni xotirada qotirib qo'yadi (pin qiladi); u endi ko'chirilolmaydi. `Box` pointer esa ko'chishi mumkin — faqat ma'lumotning o'zi qotib turadi.

### Pin va Unpin

`Unpin` — marker trait (Send, Sync kabi). Kompilyator aksariyat tiplar uchun avtomatik implement qiladi.

| | Xatti-harakat |
|---|---|
| `Unpin` implement qiladi | `Pin<T>` bilan ham ko'chirish xavfsiz |
| `!Unpin` (implement qilmaydi) | `Pin<T>` ichida qotib qolishi kerak |

Async future'lar `!Unpin` — ular self-referential bo'lishi mumkin. `String`, `Vec`, `i32` kabi oddiy tiplar `Unpin` — ichki reference yo'q.

### join_all muammosi va pin! yechimi

```rust
// XATO — E0277: dyn Future<Output=()> does not implement Unpin
let futures: Vec<Box<dyn Future<Output = ()>>> =
    vec![Box::new(tx1_fut), Box::new(rx_fut), Box::new(tx_fut)];
trpl::join_all(futures).await;
```

`join_all` uchun future'lar `Unpin` bo'lishi kerak. Yechim: `pin!` macro bilan pinlash:

```rust
use std::pin::{Pin, pin};

let tx1_fut = pin!(async move { /* ... */ });
let rx_fut  = pin!(async       { /* ... */ });
let tx_fut  = pin!(async move  { /* ... */ });

let futures: Vec<Pin<&mut dyn Future<Output = ()>>> =
    vec![tx1_fut, rx_fut, tx_fut];

trpl::join_all(futures).await;
```

`Box::pin(...)` ham ishlatilishi mumkin — tashqaridan ham `Pin<Box<T>>` ko'rinishida.

### Stream trait ta'rifi

```rust
trait Stream {
    type Item;
    fn poll_next(
        self: Pin<&mut Self>,
        cx: &mut Context<'_>,
    ) -> Poll<Option<Self::Item>>;
}
```

`Iterator::next` + `Future::poll` birikmas:
- `Option` — yana element bormi yoki stream tugadimi
- `Poll` — hozir tayyor yoki kutish kerakmi

`StreamExt` — ergonomik yuqori qatlam: `next()` (async), `map()`, `filter()`, va boshqalar. Custom `Stream` yozganda faqat `Stream`ni implement qilish yetarli — `StreamExt` avtomatik ishlaydi.

## Key Concepts

- [[pin|Pin va Unpin]] — self-referential future'larni xotirada qotirish
- [[future|Future trait]] — `poll`, `Pin<&mut Self>`, `Context`, `Poll<T>`
- [[stream|Stream trait]] — `poll_next`, Iterator+Future birikmas
- [[join-all|join_all]] — dynamic futures collection, `pin!` macro kerakligi

## Code Examples

- [[join-all-pinned]] — `pin!` macro bilan Vec ichida future'lar

## Exercises or Practice Ideas

1. `Pin<Box<T>>` va `pin!(expr)` farqini tushuntiring.
2. Nima uchun `String` `Unpin`? Async future'lar nima uchun `!Unpin`?
3. Custom `Future` implementatsiya yozing: `ImmediateFuture` — har doim `Ready`.

## Questions Raised

- `Context` va `Waker` qanday ishlaydi?
- `pin!` macro'dan keyin `futures: Vec<Pin<&mut dyn Future>>` nima uchun ishlaydi?

## Links To Update

- [[future]] — Pin, Context, Poll chuqurroq
- [[stream]] — poll_next, StreamExt
- [[pin]] — yangi concept page
