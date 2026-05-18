---
title: "Static Items"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, static, globals]
source_count: 5
---

# Static Items

## Short Definition

`static` — dastur ichida bitta aniq allocationga ega bo'ladigan, butun program davomida yashaydigan named value.

## Why It Matters

`static` oddiy `const`ga o'xshab ko'rinadi, lekin u compile-time inline qilinadigan value emas. U bitta shared storage beradi. Bu global configuration, lookup table, atomics, yoki FFI bilan ishlaganda muhim.

## Mental Model

`const`ni "har ishlatilganda qiymatni qo'yib yuboriladigan formula" deb tasavvur qilsangiz, `static`ni "dastur xotirasidagi bitta aniq joy" deb tasavvur qiling.

```rust
static LANGUAGE: &str = "Rust";
```

Bu yerda `LANGUAGE` uchun bitta shared allocation mavjud bo'ladi. U function har chaqirilganda qayta yaratilmaydi.

Muhim farq: `static` keyword va [[static-lifetime|'static lifetime]] bir xil narsa emas, lekin bog'liq. `static` item odatda `'static` lifetime bilan bog'liq ma'lumot beradi, ammo `'static` lifetime reference types va lifetime system haqidagi tushuncha.

Backend beginner source `static`ni `const`dan yana ikki yo'l bilan ajratadi: u heap-owning qiymatlarni ham ushlashi mumkin va mutable `static` concurrent mutation sabab unsafe boundary'ga kiradi.

## Syntax and Examples

Oddiy immutable static:

```rust
static APP_NAME: &str = "my-app";

fn main() {
    println!("{APP_NAME}");
}
```

Function ichida yozilgan `static` ham bitta allocation bo'ladi:

```rust
use std::sync::atomic::{AtomicUsize, Ordering};

fn next_id() -> usize {
    static COUNTER: AtomicUsize = AtomicUsize::new(0);
    COUNTER.fetch_add(1, Ordering::Relaxed)
}
```

Bu `COUNTER` har call'da qayta yaratilmaydi; hamma calllar bitta shared counterni ishlatadi.

Mutable static mavjud, lekin u xavfli:

```rust
static mut LEVELS: u32 = 0;

unsafe {
    LEVELS += 1;
}
```

`static mut` bilan ishlash `unsafe`, chunki shared mutable global state data race xavfini oshiradi. Rust 2024'da `&LEVELS` reference'ni olish `static_mut_refs` lint orqali deny qilingan; o'rniga [[raw-borrow-operators|raw borrow operators]] (`&raw const LEVELS`) ishlatish kerak. Batafsil: [[mutable-static]].

Rustda amalda ko'pincha `static mut` o'rniga `Atomic*`, `Mutex`, `RwLock`, yoki [[oncelock|OnceLock]]/[[lazylock|LazyLock]] kabi vositalar tanlanadi.

Source storage segmentlari (`data`, `bss`) haqida ham gapiradi. Bu foydali low-level mental model, lekin ABI va optimization tafsilotlari sifatida qarash to'g'riroq.

## Common Mistakes

- `static`ni oddiy `const` bilan bir xil deb o'ylash.
- `static` va [[static-lifetime|'static lifetime]]ni aynan bitta narsa deb tushunish.
- Function ichidagi `static` har call'da yangidan yaratiladi deb o'ylash.
- `static mut`ni oddiy mutable global variable sifatida xavfsiz ishlatish mumkin deb o'ylash.
- Shared global state kerak bo'lmaganda `static` ishlatish.

## Related Concepts

- [[constants]]
- [[variables-and-mutability|variables and mutability]]
- [[static-lifetime]]
- [[ownership]]
- [[scope]]
- [[data-race|data race]]
- [[mutable-static|mutable static]]
- [[unsafe-rust|unsafe Rust]]
- [[raw-borrow-operators|raw borrow operators]]
- [[lazylock|LazyLock]]
- [[oncelock|OnceLock]]
- [[global-data]]

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-variables]]
- [[wiki/sources/rust-for-backend-developers-global-data]]
- [Rust Reference: Constant items](https://doc.rust-lang.org/stable/reference/items/constant-items.html)
- [Rust Reference: Static items](https://doc.rust-lang.org/stable/reference/items/static-items.html)
