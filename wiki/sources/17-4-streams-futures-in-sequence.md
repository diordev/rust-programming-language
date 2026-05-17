---
title: "17.4. Streams: Futures in Sequence"
type: source
status: needs-review
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, streams]
source_count: 1
---

# 17.4. Streams: Futures in Sequence

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-04-streams.html
- Raw: `raw/books/the_rust_programming_language/17.4. Streams Futures in Sequence.md`

> **Eslatma**: Raw fayl qisman — web clipper sahifaning faqat bir qismini olgan (91 satr). Faqat intro va `stream_from_iter` qismi mavjud. Qolgan qism (stream combinators, `filter`, `throttle`, va h.k.) raw faylda yo'q. Status `needs-review` — to'liq ingest uchun raw fayl to'ldirilishi kerak.

## Detailed Summary

### Stream nima?

Stream — **asynchronous iterator**. Vaqt o'tishi bilan ketma-ket qiymatlar beradi. Iterator sinxron (hozir qiymat bor yoki yo'q), stream esa asinxron (keyingi qiymat kelajakda kelishi mumkin).

Amaliy misollar:
- Navbatdagi elementlar (async channel receiver)
- Fayldan o'qiladigan bo'laklar (juda katta fayl)
- Tarmoq orqali kelayotgan ma'lumotlar

### Iterator vs Stream

| | Iterator | Stream |
|---|---|---|
| Sinxronlik | Sinxron | Asinxron |
| Metod | `next()` — qiymat beradi | `next().await` — future beradi |
| Loop | `for v in iter` | `while let Some(v) = s.next().await` |
| Trait | `Iterator` | `Stream` + `StreamExt` |

### Stream va StreamExt

`Stream` — past darajali, `Iterator` + `Future` kombinatsiyasi. `StreamExt` — extension trait (Rust ekotizimida keng tarqalgan pattern), `next()` va boshqa utility methodlarni beradi.

`StreamExt` hali standart kutubxona qismida emas — ko'pchilik ekotizim cratelari o'z versiyasini taklif etadi.

### stream_from_iter va asosiy ishlatish

```rust
use trpl::StreamExt;

let values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let iter = values.iter().map(|n| n * 2);
let mut stream = trpl::stream_from_iter(iter);

while let Some(value) = stream.next().await {
    println!("The value was: {value}");
}
```

`stream.next()` `Future` qaytaradi — `.await` kerak. `StreamExt` scope'ga import qilinmasa compiler xato beradi.

## Key Concepts

- [[stream|Stream trait]] — async iterator, StreamExt
- [[iterators]] — sinxron ekvivalent
- [[async-channel|async channel]] — stream'ning oddiy misoli (async recv)

## Code Examples

- [[stream-from-iter]] — iterator'dan stream, `while let Some` pattern

## Exercises or Practice Ideas

1. `1..=10` range'ni stream'ga aylantirib chop eting.
2. Stream'ga `.filter()` adapter qo'shing.
3. Iterator `map`/`filter` vs Stream `map`/`filter` farqini tekshiring.

## Questions Raised

- `Stream` trait standart kutubxonaga qachon kiradi?
- `StreamExt` qanday utility methodlar beradi (`filter`, `map`, `take`, va h.k.)?
- Async channel receiver `Stream` implement qiladimi?

## Links To Update

- [[wiki/chapters/17-4-streams-futures-in-sequence]] — yangi chapter page
- [[stream]] — yangi concept page
