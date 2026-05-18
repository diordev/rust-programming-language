---
title: "Глобальные данные - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, backend, source, global-data, advance]
source_count: 1
---

# Глобальные данные - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/53. Глобальные данные.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/global-data.html

## Detailed Summary

Bu source global data mavzusini concurrency bobidan keyin ochadi. Mantiq to'g'ri: global state muammosi alohida syntax emas, synchronization va lifetime muammosi.

Kirish qismi `const` va `static`ni ajratadi. `const` immutable, `'static` lifetime'ga ega, va compile time'da hisoblanishi kerak. Shu sabab heap ishlatadigan `HashMap` kabi qiymatlar oddiy `const` yoki plain `static` initializer bilan har doim yozilmaydi. Source bu yerda to'g'ri signal beradi: muammo "global value kerak" emas, "bu value compile time'da qurila oladimi" va "runtime davomida o'zgaradimi" degan ikki savolda.

Keyingi qatlam global `static` items. `static VAR1: i32 = 1;` va `static mut VAR2: i32 = 2;` misoli bilan source safe immutable global va unsafe mutable global farqini ko'rsatadi. Muhim durable takeaway: `static mut` oddiy "global mutable variable" emas; Rust uni default xavfli deb ko'radi. O'qish ham, yozish ham `unsafe` talab qiladi, chunki concurrent access data race'ga olib kelishi mumkin.

Source bu yerdan natural xulosaga keladi: synchronization'siz mutable global yomon design. Bu yerda wiki caveat qo'shiladi: global state ba'zan kerak bo'lishi mumkin, lekin "global bo'lsa qulay" degan sabab bilan tanlanadigan pattern emas. `static mut` default emas.

Keyin source global variable'ni `Mutex` yoki `RwLock` bilan himoyalashni ko'rsatadi. `static COUNTER: Mutex<u64> = Mutex::new(0);` misoli muhim, chunki u ikki narsani birga ko'rsatadi: `Mutex::new` const initializer sifatida ishlaydi va scoped threads bilan global lock ham local lock kabi ishlatiladi. Threadlar `COUNTER.lock().unwrap()` orqali ishlaydi va final result `200` bo'ladi. Bu example safe global mutation patterni sifatida foydali.

Lekin `HashMap::new()` kabi non-const constructor bilan `static` initializer yozib bo'lmaydi. Source `E0015` xatosini ko'rsatib, compiler tavsiya qilgan `LazyLock`ga o'tadi. `LazyLock` value'ni first access paytida initialize qiladi. Muhim boundary: `LazyLock::new` const fn bo'lgani uchun static initializer ichida ishlaydi, lekin ichidagi closure runtime'da ishlaydi. Shunday qilib compile-time restriction bilan runtime initialization ehtiyoji orasidagi ko'prik quriladi.

`LazyLock<Mutex<HashMap<String, i32>>>` misoli source'dagi asosiy amaliy patternlardan biri. Bu "heap-owning global + synchronization" uchun standard yo'l sifatida yoziladi. Source `LazyLock`ning `Deref` implement qilishi sabab syntax transparent bo'lishini ham ko'rsatadi: `M.lock().unwrap()` ishlaydi. Double-checked locking haqidagi izoh source claim sifatida saqlanadi, lekin wiki buni public API semanticsidan ko'ra implementation detail sifatida ko'radi.

`OnceLock` keyingi step. `LazyLock` faqat bitta init closure bilan ishlaydi; agar initialization path bir nechta branch yoki call site'dan kelishi kerak bo'lsa, `OnceLock` foydaliroq. `set()` faqat birinchi marta yozadi, keyingi urinishlarni `Err(value)` bilan qaytaradi. `get_or_init()` esa lazy default init yo'li. Muhim caveat: source "get on uninitialized OnceLock leads to panic" deb aytadi. Literal metod `get()` panic qilmaydi, lekin source misolida `O.get().unwrap()` panic qiladi. Wiki'da aynan shu nuance saqlanadi; aks holda `OnceLock` API noto'g'ri hujjatlashtiriladi.

Session storage misoli backend kontekst uchun eng muhim qism. Source avval `LazyLock<RwLock<HashMap<String, UserSession>>>` modelini taklif qiladi: sessionlar ko'p o'qiladi, kam yoziladi, shuning uchun `RwLock` `Mutex`dan yaxshiroq bo'lishi mumkin. Bu to'g'ri high-level intuition, lekin bottleneck ham aniq: har request session ma'lumotiga ko'p marta murojaat qilsa, global `RwLock` read lock ham issiq nuqtaga aylanadi.

Shu muammoni kamaytirish uchun source `Arc<UserSession>`ni map ichiga qo'yadi va lock scope'ini qisqartiradi: request handler map'dan session pointer'ni tezda olib chiqadi, keyin global `RwLock`ni bo'shatadi. Bu amaliy jihatdan to'g'ri direction.

Keyin source `Arc<UserSession>` mutable emasligini ko'rsatadi va `Arc<Mutex<UserSession>>`ga o'tadi. Bu pattern'da tashqi `RwLock<HashMap<...>>` session registry uchun, ichki `Mutex<UserSession>` esa individual session mutation uchun ishlaydi. Durable takeaway: global registry lock qisqa ushlanadi; session-level mutation alohida lock bilan boshqariladi. Wiki caveat shu yerda kuchaytiriladi: `Arc<Mutex<UserSession>>` olgandan keyin lock scope iloji boricha tor bo'lishi kerak. Uzoq computation yoki I/O session mutex ichida qolmasligi kerak.

Source oxirida `unwrap()`lar simplification ekani aytiladi. Bu muhim. Global state va session store misollarida production code panic bilan emas, error-aware path bilan yozilishi kerak.

## Key Concepts

- [[global-data]]
- [[global-state]]
- [[constants]]
- [[static-items]]
- [[mutable-static]]
- [[lazylock|LazyLock]]
- [[oncelock|OnceLock]]
- [[mutex-t|Mutex<T>]]
- [[rwlock|RwLock]]
- [[arc-t|Arc<T>]]

## Code Examples

```rust
static COUNTER: std::sync::Mutex<u64> = std::sync::Mutex::new(0);
```

```rust
static M: std::sync::LazyLock<
    std::sync::Mutex<std::collections::HashMap<String, i32>>
> = std::sync::LazyLock::new(|| {
    std::sync::Mutex::new(std::collections::HashMap::new())
});
```

```rust
static O: std::sync::OnceLock<std::sync::Mutex<String>> =
    std::sync::OnceLock::new();
```

```rust
static SESSIONS: std::sync::LazyLock<
    std::sync::RwLock<
        std::collections::HashMap<String, std::sync::Arc<std::sync::Mutex<UserSession>>>
    >
> = std::sync::LazyLock::new(|| std::sync::RwLock::new(std::collections::HashMap::new()));
```

## Exercises or Practice Ideas

- `static mut` bilan oddiy counter yozib, keyin uni `AtomicUsize`, `Mutex<u64>`, va `OnceLock<Mutex<_>>` bilan qayta yozing.
- `LazyLock<HashMap<...>>` nega compile bo'lmasligini va `LazyLock<Mutex<HashMap<...>>>` nega compile bo'lishini izohlang.
- `OnceLock::set` va `get_or_init` uchun ikkita turli initialization scenario yozing.
- Session store misolida registry lock va session lock scope'larini alohida belgilang.

## Questions Raised

- Global data haqiqatan kerakmi yoki dependency injection bilan yechiladimi?
- `Mutex<T>` yetarlimi yoki read-heavy workload uchun `RwLock<T>` kerakmi?
- `Arc<Mutex<UserSession>>` ichida lock contention qayerda paydo bo'lishi mumkin?
- `LazyLock` va `OnceLock`dan qaysi biri init ownership modeliga ko'proq mos?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-global-data]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[global-data]]
- [[global-state]]
- [[lazylock|LazyLock]]
- [[oncelock|OnceLock]]
- [[constants]]
- [[static-items]]
- [[mutable-static]]
- [[mutex-t|Mutex<T>]]
- [[rwlock|RwLock]]
- [[arc-t|Arc<T>]]
