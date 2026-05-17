---
title: "17.5. A Closer Look at the Traits for Async"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, traits, pin]
source_count: 1
---

# 17.5. A Closer Look at the Traits for Async

## Learning Goal

`Future::poll`, `Pin`, `Unpin`, va `Stream` trait'larining ichki mexanizmlarini tushunish. `join_all` bilan Vec ichida future'larni ishlatishda paydo bo'ladigan Pin/Unpin xatolarini hal qila bilish.

## Main Ideas

### Future::poll — await ostida nima bor

```rust
pub trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
pub enum Poll<T> { Ready(T), Pending }
```

`await` — aslida polling loopi:

```
loop {
    match future.poll(cx) {
        Ready(val) => break val,   // bajarildi
        Pending    => yield,       // runtime boshqalarga o'tadi
    }
}
```

Runtime blocking loop emas — `Pending` kelganda runtime boshqa task'ga o'tadi, keyinroq qaytadi.

### Nima uchun Pin kerak?

Async state machine'ning ba'zi variantlari **o'z-o'ziga reference** saqlaydi:

```
Future state machine:
  TextAwaitPoint {
      response: trpl::Response,
      // + ichki reference → response ichidagi ma'lumotga
  }
```

Bu tur xotirada ko'chirilsa, ichki reference eski manzilga ko'rsatib qoladi — **dangling reference**. `Pin<Box<T>>` kafolati: `T` xotirada qotadi, ko'chirilmaydi.

```
Pin --→ Box --→ [Future ma'lumotlari (qotgan)]
                      ↑
              ichki reference ham shu yerga ko'rsatadi — xavfsiz
```

### Unpin — "ko'chirish xavfsiz" markeri

Aksariyat tiplar `Unpin` — ichki reference yo'q, ko'chirish xavfsiz:

```rust
String // Unpin ✓
Vec<T> // Unpin ✓
i32    // Unpin ✓
async future  // !Unpin — self-referential bo'lishi mumkin
```

`Unpin` implement qilmagan (`!Unpin`) tip uchun `Pin<T>` haqiqatan qotirib qo'yadi.

### join_all muammosi va pin! yechimi

```rust
// XATO E0277:
let futures: Vec<Box<dyn Future<Output = ()>>> =
    vec![Box::new(fut1), Box::new(fut2)];
trpl::join_all(futures).await;
// "dyn Future<Output=()> does not implement Unpin"
```

Yechim — `pin!` macro:

```rust
use std::pin::{Pin, pin};

let fut1 = pin!(async move { /* ... */ });
let fut2 = pin!(async       { /* ... */ });

let futures: Vec<Pin<&mut dyn Future<Output = ()>>> =
    vec![fut1, fut2];
trpl::join_all(futures).await;
```

`pin!` future'ni joriy stack'da pinlaydi. `Box::pin(...)` esa heap'da pinlaydi — tashqaridan qaytarish kerak bo'lsa.

### Stream trait — Iterator + Future birikmas

```rust
trait Stream {
    type Item;
    fn poll_next(
        self: Pin<&mut Self>,
        cx: &mut Context<'_>,
    ) -> Poll<Option<Self::Item>>;
}
```

- `Option` — Iterator'dan: element bor yoki stream tugadi
- `Poll` — Future'dan: hozir tayyor yoki kutish kerak

`StreamExt` avtomatik barcha `Stream` implementatsiyalari uchun ishlaydi — custom `Stream` yozganda `poll_next`ni implement qilish yetarli.

## Concepts

- [[pin|Pin va Unpin]] — self-referential future'lar, xotirada qotirish
- [[future|Future trait]] — `poll`, `Pin<&mut Self>`, `Context`, `Poll`
- [[stream|Stream trait]] — `poll_next`, StreamExt munosabati
- [[join-all|join_all]] — dynamic futures Vec, Pin kerakligi
- [[async-state-machine|async state machine]] — self-referential variantlar

## Examples

- [[join-all-pinned]] — `pin!` macro bilan Vec ichida future'lar

## Exercises

1. `Pin<Box<T>>` va `pin!(expr)` orasidagi farqni kod bilan ko'rsating.
2. `String` nima uchun `Unpin`? `async` future nima uchun `!Unpin`? Tushuntiring.
3. `ImmediateFuture` — doimo `Ready` qaytaradigan custom `Future` yozing.

## Review Questions

1. `Future::poll` nima qaytaradi va `Pending`da runtime nima qiladi?
2. Self-referential future ko'chirilsa nima yuz beradi?
3. `Unpin` marker trait qanday yordam beradi?
4. `join_all` nima uchun `Pin` talab qiladi?
5. `Stream::poll_next` va `Future::poll` qanday farqlanadi?

## Related Pages

- [[17-1-futures-and-async-syntax|17.1]] — Future'ning birinchi tanishuvi
- [[wiki/chapters/17-4-streams-futures-in-sequence|17.4]] — Stream ishlatish
- [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async|Source: 17.5]]
