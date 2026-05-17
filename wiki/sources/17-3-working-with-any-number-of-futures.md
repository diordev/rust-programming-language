---
title: "17.3. Working With Any Number of Futures"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# 17.3. Working With Any Number of Futures

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-03-more-futures.html
- Raw: `raw/books/the_rust_programming_language/17.3. Working With Any Number of Futures.md`

> **Eslatma**: Raw fayl `yield_now` bo'limidan boshlanadi. Kitobning boshi (`join_all` va N ta future bilan ishlash) raw faylda yo'q — web clipper bo'limni to'liq keltirmagan.

## Detailed Summary

Bu bo'lim ikkita muhim mavzuni ko'rib chiqadi: runtime'ga boshqaruvni qanday qaytarish va async abstraktsiyalarni qanday compose qilish.

### Starvation — future'larni "ochlik"

`async` blok ichida `await` nuqtasigacha barcha kod **sinxron** bajariladi. Bir future uzoq vaqt `await` qilmasdan ishlasa, boshqa future'lar progress qila olmaydi — bu **starvation** (ochlik) deyiladi:

```rust
let a = async {
    slow("a", 30);  // thread::sleep — await yo'q!
    slow("a", 10);
    slow("a", 20);
    trpl::sleep(50ms).await;  // faqat shu yerda boshqaruv qaytariladi
};
// 'a' barcha slow() ishlarini tugataguncha 'b' boshlanmaydi
```

### trpl::yield_now — cooperative yielding

`trpl::yield_now().await` — darhol runtime'ga boshqaruvni qaytaradi, hech qancha kutmasdan:

```rust
slow("a", 30);
trpl::yield_now().await;   // runtime'ga: "menga navbat berishing mumkin"
slow("a", 10);
trpl::yield_now().await;
```

`trpl::sleep(1ms).await` dan yaxshiroq: `sleep` minimal 1ms kutadi; `yield_now` esa hech kutmaydi — shunchaki navbatni beradi.

### Cooperative multitasking

`yield_now` — **kooperativ ko'p vazifali** (cooperative multitasking) ning asosidir. Har bir future qachon boshqaruvni qaytarishini o'zi belgilaydi. Thread'lar (preemptive) bilan farqi:
- **Preemptive**: OS ixtiyoriy vaqtda thread'ni to'xtatadi
- **Cooperative**: future o'zi `await` nuqtasida boshqaruvni qaytaradi

Bu sababli CPU-bound vazifalar `yield_now` bilan bo'lsa ham async bilan yaxshi ishlashi mumkin.

### Custom async abstraktsiyalar — timeout

`select` va `sleep` yordamida `timeout` funksiyasini qurilishi kuchli namuna:

```rust
async fn timeout<F: Future>(
    future_to_try: F,
    max_time: Duration,
) -> Result<F::Output, Duration> {
    match trpl::select(future_to_try, trpl::sleep(max_time)).await {
        Either::Left(output) => Ok(output),
        Either::Right(_)     => Err(max_time),
    }
}
```

Ishlatish:

```rust
match timeout(slow_future, Duration::from_secs(2)).await {
    Ok(msg)      => println!("Tugadi: {msg}"),
    Err(elapsed) => println!("Timeout: {}s", elapsed.as_secs()),
}
```

`select` first argument'ni birinchi poll qiladi — shuning uchun `future_to_try` birinchi o'rinda turadi va very short timeout bo'lsa ham imkon beriladi.

Composability: `timeout` + `retry` + network call — kichik async building block'lardan katta abstraktsiya qurish mumkin.

## Key Concepts

- [[yield-now|yield_now]] — cooperative yielding, starvation oldini olish
- [[async-timeout|async timeout]] — select + sleep bilan custom timeout
- [[async-state-machine|async state machine]] — await'siz kod bloklar arasi parallel emas
- [[future|Future trait]] — `async fn timeout<F: Future>` generic pattern

## Code Examples

- [[async-timeout]] — `timeout` funksiyasi implementatsiyasi

## Exercises or Practice Ideas

1. `yield_now` bilan ikki starvation holatini solishtiring: `yield_now` va `sleep(1ms)`.
2. `timeout` funksiyasini retry logikasi bilan kengaytiring: timeout bo'lsa N marta qayta urinsin.
3. `timeout` bilan `trpl::select` farqini tushuntiring: qachon qaysi?

## Questions Raised

- `trpl::select` birinchi argumentni birinchi poll qilishi bias yaratadimi?
- `yield_now` real world da qachon kerak? CPU-bound task async runtime'da yaxshi ushlab turadimi?

## Links To Update

- [[wiki/chapters/17-3-working-with-any-number-of-futures]] — yangi chapter page
- [[yield-now]] — yangi concept page
