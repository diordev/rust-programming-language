---
title: "Stream"
type: concept
status: needs-review
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, streams]
source_count: 2
---

# Stream

> **Eslatma**: Raw fayl 17.4 qisman. Bu sahifa to'ldirilishi kerak. Status `needs-review`.

## Short Definition

Stream — **asynchronous iterator**: vaqt o'tishi bilan ketma-ket qiymatlar beradi. `Iterator` sinxron bo'lsa, `Stream` asinxron — har bir `next()` future qaytaradi.

## Why It Matters

Ko'pgina haqiqiy dunyo ma'lumot manbalari vaqt o'tishi bilan keladi: tarmoq paketlari, fayldan o'qish, event queue'lar. Stream bu "vaqt bo'yicha ketma-ket qiymatlar" tushunchasini `Future` bilan birlashtirib ifodalaydi. Stream'lar future ekotizimi bilan to'liq integratsiyalashgan — timeout, select, join kabi operatorlar bilan ishlatilishi mumkin.

## Mental Model

Iterator → sinxron konveyer lentasi: barcha elementlar hozir mavjud.
Stream → asinxron konveyer lentasi: elementlar vaqt o'tishi bilan keladi, ba'zida kutish kerak.

Async channel receiver `while let Some(v) = rx.recv().await` — eng oddiy stream namunasi. Har `recv` call stream'ning `next()` ga teng.

## Syntax and Examples

### stream_from_iter — iterator'dan stream

```rust
use trpl::StreamExt;  // next() metodi shu yerdan

let values = [1, 2, 3, 4, 5];
let iter = values.iter().map(|n| n * 2);
let mut stream = trpl::stream_from_iter(iter);

while let Some(value) = stream.next().await {
    println!("{value}");
}
```

`stream.next()` — `Future<Output = Option<T>>` qaytaradi:
- `Some(value)` — element bor
- `None` — stream tugadi

### StreamExt import

```rust
// StreamExt import qilinmasa compiler xato beradi:
// error[E0599]: no method named `next` found for struct `Iter`
use trpl::StreamExt;  // yoki futures_util::stream::stream::StreamExt
```

### Stream trait tuzilishi (soddalashtirilgan)

```rust
pub trait Stream {
    type Item;
    fn poll_next(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Option<Self::Item>>;
}
```

`StreamExt` esa `next()`, `map()`, `filter()`, `take()` kabi yuqori darajali metodlar beradi.

## Common Mistakes

- **`StreamExt` import qilmaslik**: `next()` metodi `StreamExt`da, `Stream`da emas.
- **`stream.next()` ga `.await` unutish**: Future qaytaradi, lekin `.await` bo'lmasa hech narsa bajarmaydi.
- **`stream` mut bo'lmasligi**: Iterator kabi stream `mut` bo'lishi kerak.

## Related Concepts

- [[iterators]] — sinxron ekvivalent
- [[future|Future trait]] — stream'ning har bir elementi future
- [[async-channel|async channel]] — stream'ning oddiy namunasi
- [[async-await|async/await]] — stream'ni ishlatish sintaksisi

## Sources

- [[wiki/sources/17-4-streams-futures-in-sequence]]
- [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async]]
