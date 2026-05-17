---
title: "join_all"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# join_all

## Short Definition

`join_all` — runtime'da sonini bilmagan future'lar to'plamini (Vec, iterator) birgalikda bajaruvchi funksiya. `trpl::join!` macro compile-time soni ma'lum bo'lganda ishlaydi; `join_all` esa runtime'da dinamik to'plamlar uchun.

## Why It Matters

Ko'p hollarda future'lar soni compile-time'da ma'lum emas: URL'lar ro'yxati fayldan o'qilishi, API'dan kelishi mumkin. `join!` macro bunday holat uchun mos emas. `join_all` esa `Vec<F>` yoki iterator qabul qilib, barchasini birgalikda bajaradi.

## Mental Model

`join_all` — parallel ish buyrug'i: "ro'yxatdagi barcha ishlarni bir vaqtda bajar, hammasi tugaganda qaytgin". `join!`dan farqi: soni compile-time'da belgilanmaydi.

| | `trpl::join!` | `join_all` |
|---|---|---|
| Future soni | Compile-time ma'lum | Runtime'da dinamik |
| Sintaksis | Macro | Funksiya |
| Input | Ko'p argument | `Vec<F>` yoki iterator |
| Output | `(A, B, C, ...)` tuple | `Vec<T>` |

## Syntax and Examples

### Oddiy ishlatish

```rust
use futures::future::join_all;

let urls = vec!["https://a.com", "https://b.com", "https://c.com"];
let futures: Vec<_> = urls.iter().map(|url| fetch_title(url)).collect();

let results: Vec<Option<String>> = join_all(futures).await;
```

### pin! bilan Vec ichida heterogen future'lar

Turli xil anonymous async bloklar bitta `Vec`ga solinsa, `Box<dyn Future>` yoki `Pin<&mut dyn Future>` kerak:

```rust
use std::pin::{Pin, pin};

let fut1 = pin!(async move { /* tx1 */ });
let fut2 = pin!(async       { /* rx  */ });
let fut3 = pin!(async move  { /* tx2 */ });

let futures: Vec<Pin<&mut dyn Future<Output = ()>>> =
    vec![fut1, fut2, fut3];

trpl::join_all(futures).await;
```

### Nima uchun Pin kerak?

`join_all` quyidagi bound talab qiladi:
```
JoinAll<F> where F: Future
Box<dyn Future<Output=()>>: Future  ← faqat agar T: Unpin
```

Async future'lar `!Unpin` (self-referential). Shuning uchun:
- `Box<dyn Future>` emas → `Box::pin(...)` yoki `pin!(...)`
- `Vec<Box<dyn Future>>` emas → `Vec<Pin<&mut dyn Future>>` yoki `Vec<Pin<Box<dyn Future>>>`

## Common Mistakes

- **`Vec<Box<dyn Future>>`** ishlatish: E0277 xatosi — `Unpin` implement qilinmagan.
- **join! vs join_all aralashtirib yuborish**: compile-time soni ma'lum bo'lsa `join!`, dinamik bo'lsa `join_all`.

## Related Concepts

- [[async-join|async join]] — `trpl::join`, `trpl::join!`
- [[pin|Pin va Unpin]] — join_all uchun Pin zarurligi
- [[future|Future trait]] — join_all qabul qiladigan tur

## Sources

- [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async]]
