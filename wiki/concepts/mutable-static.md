---
title: "Mutable Static Variable"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, unsafe, globals, concurrency]
source_count: 2
---

# Mutable Static Variable

## Short Definition

`static mut NAME: T = ...;` — global mutable o'zgaruvchi. O'qish va yozish ikkalasi `unsafe` talab qiladi, chunki thread'lararo data race xavfi bor.

## Why It Matters

Global mutable state — ko'p tillarda oddiy, lekin Rust'da xavfli. Concurrency'da deyarli har doim noto'g'ri tanlov; o'rniga `Mutex`, `RwLock`, [[oncelock|OnceLock]], yoki [[lazylock|LazyLock]] ishlatish kerak. Lekin ba'zan FFI yoki low-level kodda majbur bo'ladi.

## Mental Model

```
static     →  immutable global, har joyda safe o'qish
const      →  compile-time, qiymat har ishlatishda inline
static mut →  runtime mutable, faqat unsafe blokda
```

`static` qiymati **bitta xotira manzilida** yashaydi (uniqud); `const` esa har ishlatishda nusxa olishi mumkin.

## Syntax and Examples

### Immutable static (xavfsiz)

```rust
static HELLO_WORLD: &str = "Hello, world!";

fn main() {
    println!("{HELLO_WORLD}");   // safe
}
```

### Mutable static (xavfli)

```rust
static mut COUNTER: u32 = 0;

/// SAFETY: Calling this from more than a single thread at a time is UB,
/// so you *must* guarantee single-threaded access.
unsafe fn add_to_count(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}

fn main() {
    unsafe {
        // SAFETY: only main thread.
        add_to_count(3);
        println!("COUNTER: {}", *(&raw const COUNTER));
    }
}
```

E'tibor:
- O'qish ham, yozish ham `unsafe` blokda.
- `&COUNTER` — `static_mut_refs` lint tomonidan ogohlantirish/xato. [[raw-borrow-operators|raw borrow operator]] ishlatish kerak: `*(&raw const COUNTER)`.
- `SAFETY:` izohi caller javobgarligini hujjatlaydi.

### `const` vs `static` farqi

| | `const` | `static` |
|---|---------|----------|
| Xotira manzili | Inlined, nusxa | Bitta manzil |
| Mutability | Hech qachon | `mut` bilan mumkin |
| Reference | `&CONST` lifetime mezbon | `&STATIC` always `'static` |
| Foydalanish | Compile-time qiymatlar | Global xotira slots |

## Xavfsiz Alternativalar

| Maqsad | Tavsiya |
|--------|---------|
| Bitta marta init, ko'p o'qish | `OnceLock<T>` |
| Lazy init | `LazyLock<T>` |
| Ko'p thread'da mutable | `Mutex<T>` yoki `RwLock<T>` |
| Counter, atomic op | `AtomicU32`, `AtomicUsize` |

```rust
use std::sync::atomic::{AtomicU32, Ordering};
static COUNTER: AtomicU32 = AtomicU32::new(0);

fn add(n: u32) {
    COUNTER.fetch_add(n, Ordering::Relaxed);
}

fn read() -> u32 {
    COUNTER.load(Ordering::Relaxed)
}
```

Bu yechim `unsafe`siz va thread-safe.

## Common Mistakes

- **`static mut` ga reference olish (`&COUNTER`).** `static_mut_refs` lint xato qiladi (Rust 2024 da deny). Raw borrow operator ishlating.
- **Multi-thread'da `static mut` ishlatish.** Data race → UB. `Atomic*` yoki `Mutex` ishlating.
- **`const` o'rniga `static mut` ishlatish.** Compile-time qiymatlar uchun `const` etarli.
- **`static mut`'ni initialize qilishda kompleks ifoda kutmoqchi bo'lish.** `static` faqat `const fn` natijalari yoki literal qiymatlardan iborat bo'lishi kerak.

## Related Concepts

- [[static-items|static items]] — umumiy holatlar
- [[static-lifetime|'static lifetime]] — har static `'static`
- [[unsafe-rust|unsafe Rust]]
- [[raw-borrow-operators|raw borrow operators]] — o'qish/yozish uchun
- [[data-race|data race]] — multi-thread xavf
- [[constants]] — alternativ
- [[concurrency]] — `Mutex`/`Atomic` xavfsiz alternativalari
- [[mutex-t|Mutex<T>]]
- [[lazylock|LazyLock]]
- [[oncelock|OnceLock]]
- [[global-state]]

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-global-data]]
