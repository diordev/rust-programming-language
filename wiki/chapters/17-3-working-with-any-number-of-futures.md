---
title: "17.3. Working With Any Number of Futures"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# 17.3. Working With Any Number of Futures

## Learning Goal

Async runtime'ga kooperativ tarzda boshqaruvni qaytarishni (`yield_now`) va kichik async building block'lardan katta abstraktsiya (`timeout`) qurish texnikasini o'rganish.

## Main Ideas

### await nuqtalari orasidagi kod sinxron

`async {}` blok ichida `await` nuqtasigacha barcha kod sinxron bajariladi — runtime aralasha olmaydi:

```
[await]--[sinxron]--[sinxron]--[sinxron]--[await]
                                         ^
                               faqat shu yerda runtime kiradi
```

`await` nuqtasi ko'p bo'lmasa, bir future boshqalarni "ochlik"ka mahkum etadi (starvation).

### Starvation misoli

```rust
let a = async {
    slow("a", 30);   // await yo'q — 'b' boshlanmaydi
    slow("a", 10);
    slow("a", 20);
    trpl::sleep(50ms).await;  // faqat shu yerda b ishlashi mumkin
    println!("'a' finished");
};
// Natija: a barcha slow() lar bo'yicha yuradi, keyin b boshlanadi
```

### trpl::yield_now — explicit yielding

```rust
slow("a", 30);
trpl::yield_now().await;   // runtime'ga: "boshqalar ishlashi mumkin"
slow("a", 10);
trpl::yield_now().await;
slow("a", 20);
trpl::yield_now().await;
// Natija: a va b interleaved bo'ladi
```

`sleep(1ms)` dan yaxshiroq: timer granularity chegarasi yo'q, darhol boshqaruv qaytariladi.

### Cooperative multitasking

Async Rust **kooperativ ko'p vazifali** model:
- Har bir future o'zi `await` da boshqaruvni qaytaradi
- OS thread'larida esa OS ixtiyoriy to'xtatadi (preemptive)

```
Preemptive:   OS istagan vaqt to'xtatadi → future bilmaydi
Cooperative:  future o'zi yield qiladi   → future nazorat qiladi
```

CPU-bound ish qilayotgan async kod ham `yield_now` bilan kooperativ bo'lishi mumkin. Ba'zi embedded OS'lar faqat cooperative multitasking'ni qo'llaydi.

### Custom async abstraktsiyalar: timeout

Mavjud building block'lardan yangi abstraktsiya qurish:

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

Pattern:
- `future_to_try` + `sleep(max_time)` → `trpl::select` → birinchi g'olib
- `Left` → future g'olib → `Ok(result)`
- `Right` → sleep g'olib → `Err(duration)`

`select` birinchi argumentni birinchi poll qiladi (fair emas) — shuning uchun `future_to_try` birinchi o'rinda.

Ishlatish:

```rust
match timeout(my_future, Duration::from_secs(2)).await {
    Ok(result) => println!("Muvaffaqiyat: {result}"),
    Err(d)     => println!("Timeout: {}s", d.as_secs()),
}
```

Composability: `timeout` + `retry` → kuchliroq abstraksiya.

## Concepts

- [[yield-now|yield_now]] — cooperative yielding, starvation oldini olish
- [[async-state-machine|async state machine]] — await'siz kod sinxron bajariladi
- [[future|Future trait]] — `async fn timeout<F: Future>` generic
- [[async-runtime|async runtime]] — cooperative multitasking'ni boshqaradi

## Examples

- [[async-timeout]] — `timeout` funksiyasi implementatsiyasi

## Exercises

1. Starvation'ni o'z kodingizda qayta ishlab chiqaring: `await`siz uzoq loop yozing.
2. `yield_now` ni `sleep(1ns)` bilan almashtiring va tezlikni solishtiring.
3. `timeout` + retry: timeout bo'lsa 3 marta qayta urinish qo'shing.

## Review Questions

1. `await` nuqtasi orasidagi kod nima uchun sinxron bajariladi?
2. `yield_now` va `sleep(1ms)` qanday farqlanadi?
3. Cooperative vs preemptive multitasking — async Rust qaysi modelda?
4. `timeout` implementatsiyasida nima uchun `future_to_try` birinchi argument?

## Related Pages

- [[17-1-futures-and-async-syntax|17.1. Futures and the Async Syntax]]
- [[wiki/chapters/17-2-applying-concurrency-with-async|17.2. Applying Concurrency with Async]]
- [[wiki/sources/17-3-working-with-any-number-of-futures|Source: 17.3]]
