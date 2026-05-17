---
title: "17.4. Streams: Futures in Sequence"
type: chapter
status: needs-review
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, streams]
source_count: 1
---

# 17.4. Streams: Futures in Sequence

> **Eslatma**: Raw fayl qisman (91 satr). Faqat stream intro va `stream_from_iter` mavjud. Status `needs-review` — raw fayl to'ldirilgach yangilanadi.

## Learning Goal

Stream'ni async iterator sifatida tushunish: `Stream`/`StreamExt` trait'lari, iterator'dan stream yaratish, va `while let Some(v) = stream.next().await` pattern'i.

## Main Ideas

### Stream — async iterator

Stream vaqt o'tishi bilan ketma-ket qiymatlar beradi — aynan iterator'ning async versiyasi:

```
Iterator: [1, 2, 3, 4, 5]       — barchasi hozir mavjud, sinxron
Stream:   [1 ...wait... 2 ...wait... 3]  — vaqt o'tishi bilan, asinxron
```

Amaliy holatlar:
- Tarmoqdan kelayotgan paketlar
- Fayl bo'laklari (streaming read)
- Async channel receiver — `while let Some(v) = rx.recv().await`

### Iterator vs Stream farqi

| | Iterator | Stream |
|---|---|---|
| Sinxronlik | Sinxron | Asinxron |
| Asosiy metod | `next() -> Option<T>` | `next() -> impl Future<Output=Option<T>>` |
| Loop pattern | `for v in iter` | `while let Some(v) = s.next().await` |
| Trait | `Iterator` | `Stream` + `StreamExt` |

### Stream va StreamExt

`Stream` — past darajali trait (`Iterator` + `Future` birikmas). `StreamExt` — extension pattern orqali `next()` va utility metodlar (`map`, `filter`, `take` va h.k.) beradi.

`Ext` suffix Rust ekotizimida keng tarqalgan: bitta trait yadro interfeysini, `XxxExt` esa qulay yuqori darajali metodlarni ta'minlaydi.

```rust
use trpl::StreamExt;  // next() va boshqa metodlar shu yerdan

while let Some(value) = stream.next().await {
    println!("{value}");
}
```

### stream_from_iter — iterator'dan stream

```rust
use trpl::StreamExt;

let values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let iter = values.iter().map(|n| n * 2);
let mut stream = trpl::stream_from_iter(iter);

while let Some(value) = stream.next().await {
    println!("The value was: {value}");
}
```

`stream.next()` — future qaytaradi → `.await` kerak. Stream `mut` bo'lishi kerak (iterator kabi).

## Concepts

- [[stream|Stream trait]] — StreamExt, async iteration
- [[iterators]] — sinxron ekvivalent
- [[async-channel|async channel]] — stream'ning oddiy instansiyasi

## Examples

- [[stream-from-iter]] — iterator'dan stream, `while let Some` pattern

## Exercises

1. `1..=20` range'ni stream'ga aylantirib faqat juft sonlarni chop eting.
2. Stream'ga `.take(5)` adapter qo'shing.
3. Async channel receiver'ini stream sifatida ishlatishni o'rganing.

## Review Questions

1. Stream iterator'dan qanday farqlanadi?
2. Nima uchun `StreamExt` import qilish kerak?
3. `stream.next()` nima qaytaradi va nima uchun `.await` kerak?

## Related Pages

- [[wiki/chapters/17-3-working-with-any-number-of-futures|17.3. Working With Any Number of Futures]]
- [[wiki/sources/17-4-streams-futures-in-sequence|Source: 17.4]]
