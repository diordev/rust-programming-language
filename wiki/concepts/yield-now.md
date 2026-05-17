---
title: "yield_now"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# yield_now

## Short Definition

`trpl::yield_now().await` — async runtime'ga darhol boshqaruvni qaytaradi, hech qancha kutmasdan. Boshqa tayyorroq future'lar ishlashiga yo'l ochadi.

## Why It Matters

Async blok ichida `await` nuqtasigacha hamma kod sinxron bajariladi. Agar bir future uzoq vaqt `await`siz ishlasa, boshqa future'lar "och" qoladi — **starvation**. `yield_now` bu muammoni hal qiladi: CPU-bound ishni bajarayotgan future'lar orasida kooperativ navbat berish imkonini beradi.

## Mental Model

Yo'ldagi svetofor — `yield_now` yashil chiroq kabi. "Men bir oz ish qildim, endi boshqalar o'tib olsin" deydi. `sleep` esa: "Men X millisekund dam olaman" — farq bor.

Oshxonadagi navbat:
- `yield_now` → "Navbatingiz, men kutaman"
- `sleep(1ms)` → "1 daqiqa keyin qayting"

Birinchisi tezroq va resurs tejamkorroq.

## Syntax and Examples

### Asosiy ishlatish

```rust
trpl::yield_now().await;
```

### Starvation holati vs yield_now

```rust
// STARVATION — b future 'a' tugamaguncha boshlanmaydi:
let a = async {
    slow_work_1();  // await yo'q
    slow_work_2();  // await yo'q
    slow_work_3();  // await yo'q
    trpl::sleep(50ms).await;  // faqat shu yield qiladi
};

// YAXSHI — interleaved:
let a = async {
    slow_work_1();
    trpl::yield_now().await;  // b ishlashi mumkin
    slow_work_2();
    trpl::yield_now().await;
    slow_work_3();
    trpl::yield_now().await;
};
```

### yield_now vs sleep(1ms)

```rust
// Sleep: minimal 1ms kutadi (timer granularity chegarasi bor)
trpl::sleep(Duration::from_millis(1)).await;

// yield_now: hech kutmaydi — shunchaki navbatni beradi
trpl::yield_now().await;
```

`yield_now` ancha tez, chunki timer yaratmaydi va minimal vaqt chegarasi yo'q. Zamonaviy protsessorlar 1ms'da milliardlab operatsiya bajaradi.

## Common Mistakes

- **`yield_now` ni `sleep` o'rnida ishlatish**: `yield_now` kutmaydi — faqat navbat beradi. Haqiqiy kutish uchun `sleep` kerak.
- **Har bir satrga `yield_now` qo'shish**: Performance'ni yomonlashtirishi mumkin — har bir `yield` state machine transition. Kerakli joyga qo'yish yetarli.
- **CPU-bound ish uchun yagona yechim deb bilish**: Juda og'ir CPU ishni async runtime ichida qilsangiz, `spawn_blocking` ko'proq mos.

## Cooperative Multitasking

`yield_now` async Rust'ning **kooperativ ko'p vazifali** modelini ochib beradi:
- **Preemptive** (OS thread'lar): OS ixtiyoriy vaqtda thread'ni to'xtatadi
- **Cooperative** (async): future o'zi `await` da boshqaruvni qaytaradi

Ba'zi embedded OS'lar faqat kooperativ modelni qo'llaydi. Rust async ham shu modelda ishlaydi.

## Related Concepts

- [[async-state-machine|async state machine]] — har yield → state transition
- [[async-runtime|async runtime]] — yield'dan keyin boshqa future'ni poll qiladi
- [[future|Future trait]] — `yield_now` ham future qaytaradi
- [[io-bound|I/O-bound]] — I/O await punktlari tabiiy yield nuqtalari
- [[cpu-bound|CPU-bound]] — yield_now CPU-bound ish uchun ham kerak bo'ladi

## Sources

- [[wiki/sources/17-3-working-with-any-number-of-futures]]
