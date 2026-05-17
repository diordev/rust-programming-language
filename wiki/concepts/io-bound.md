---
title: "I/O-bound"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, performance, async]
source_count: 1
---

# I/O-bound

## Short Definition

I/O-bound operatsiya — ishlash tezligi kirish/chiqish (Input/Output) tezligi bilan chegaralangan ish. CPU ko'p vaqt ma'lumot kelishini kutadi: tarmoq, disk, klaviatura, va hokazo.

## Why It Matters

I/O-bound operatsiyalar async programmingning asosiy qo'llanish sohasi. CPU bekor turganda boshqa ishlarni bajarish — aynan shu async/await beradi. Web server'lar, fayllar bilan ishlash, API so'rovlar — barchasi I/O-bound.

## Mental Model

Tasavvur qiling: internet orqali katta fayl yuklab olishni. Kompyuter tarmoqdan paket-paket ma'lumot kutadi. Ma'lumot kelmagan vaqtda CPU mutlaqo bekor. Shu "bekor vaqt"da boshqa yuklab olishlarni boshlash yoki boshqa vazifalarni bajarish mumkin — aynan shunday ishlaydi async.

Qarama-qarshi holat: matematikani hisoblash — CPU hech qachon kutmaydi. Bu [[cpu-bound|CPU-bound]].

## Syntax and Examples

```rust
use std::time::Duration;

// I/O-bound — network so'rov
async fn fetch_page(url: &str) -> String {
    // CPU bu yerda kutadi, runtime boshqa ishlarni qilishi mumkin
    trpl::get(url).await.text().await
}

// Ko'p so'rovni bir vaqtda bajarish
async fn fetch_all(urls: Vec<&str>) -> Vec<String> {
    let futures: Vec<_> = urls.iter()
        .map(|url| fetch_page(url))
        .collect();

    // futures::future::join_all bilan parallel kutish
    futures::future::join_all(futures).await
}
```

Async I/O blocking versiyasi bilan solishtirish:

```rust
// Blocking — har biri navbat bilan kutadi (sekin)
fn blocking_fetch_all(urls: &[&str]) -> Vec<String> {
    urls.iter()
        .map(|url| blocking_get(url)) // har biri kutadi
        .collect()
}

// Async — barchasi parallel kutadi (tez)
async fn async_fetch_all(urls: &[&str]) -> Vec<String> {
    let futs: Vec<_> = urls.iter().map(|u| fetch_page(u)).collect();
    futures::future::join_all(futs).await
}
```

## Common Mistakes

- **I/O-bound ishni thread bilan hal qilmoq**: Thread'lar ham ishlaydi, lekin async ancha samarali — ko'p mingta concurrent so'rov uchun thread overhead katta bo'ladi.
- **Blocking I/O'ni async runtime ichida chaqirish**: `std::fs::read_to_string` kabi blocking funksiyalar async runtime thread'ini to'sib qo'yadi. Buning o'rniga `tokio::fs::read_to_string` kabi async versiyalari ishlatiladi.

```rust
// Xato: blocking fayl o'qish async kontekstda
async fn bad_read(path: &str) -> String {
    std::fs::read_to_string(path).unwrap() // runtime thread'ini to'sadi!
}

// To'g'ri: async fayl o'qish
async fn good_read(path: &str) -> String {
    tokio::fs::read_to_string(path).await.unwrap()
}
```

## Related Concepts

- [[cpu-bound|CPU-bound]] — hisoblash bilan chegaralangan operatsiya
- [[async-await|async/await]] — I/O-bound uchun asosiy yondashuv
- [[async-runtime|async runtime]] — I/O kutish vaqtida boshqa future'larni ishlatadi
- [[future|Future trait]] — I/O operatsiyasi future sifatida ifodalanadi
- [[concurrency]] — concurrent I/O operatsiyalari

## Sources

- [[17-fundamentals-of-asynchronous-programming]]
