---
title: "Unsafe Rust"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, unsafe, ffi, low-level]
source_count: 4
---

# Unsafe Rust

## Short Definition

Rust ichidagi "ikkinchi til" — `unsafe` blok orqali kompilyator tekshira olmaydigan **5 ta operatsiyaga** ruxsat beradi. Boshqa hech narsa o'zgarmaydi: borrow checker, type checker, lifetime'lar — hammasi davom etadi.

## Why It Matters

Static analysis konservativ — kompilyator ba'zi to'g'ri kodni rad etadi. Shuningdek, hardware tabiatan unsafe (OS API, memory-mapped I/O, embedded). `unsafe` shu joylarda kerak, lekin uni minimal scope'da, [[safe-abstraction|safe abstraction]] ostida saqlash kerak.

## Mental Model

`unsafe` "tekshiruvlarni o'chirish" emas — bu aniq, kichik miqdordagi qobiliyatlar to'plamini ochish:

```rust
unsafe {
    // Faqat 5 ta superpower'ning qaysi biri kerak bo'lsa shu yerda.
    // Boshqa Rust kodi tekshiruvi shu yerda ham ishlaydi.
}
```

`unsafe` blok = "Men shu yerda invariantlarni o'zim saqlayman" deb va'da berish.

Back-end application codeida `unsafe` odatda kam uchraydi. Kerak bo'ladigan joylar ko'proq [[ffi|FFI]], custom low-level integration, yoki safe abstraction ichidagi optimizatsiya bilan bog'liq.

## 5 ta Unsafe Superpowers

| # | Superpower | Sahifa |
|---|------------|--------|
| 1 | [[raw-pointer|Raw pointer]] dereference | `unsafe { *r }` |
| 2 | Unsafe function/method chaqirish | `unsafe { dangerous() }` |
| 3 | [[mutable-static|Mutable static]] ga kirish | `unsafe { COUNTER += 1 }` |
| 4 | [[unsafe-trait|Unsafe trait]] implement qilish | `unsafe impl Send for MyType {}` |
| 5 | [[union-type|Union]] field'iga kirish | `unsafe { u.field }` |

Batafsil: [[unsafe-superpowers]].

## Syntax and Examples

### Asosiy

```rust
unsafe {
    // Unsafe operatsiya shu blok ichida.
}
```

### Unsafe funksiya

```rust
unsafe fn dangerous() {}

fn main() {
    unsafe { dangerous(); }
}
```

Rust 2024'dan: `unsafe fn` ichida ham `unsafe` blok kerak (warn-by-default).

Backend pointer source bu boundaryni juda amaliy ko'rsatadi: pointer yaratish safe bo'lishi mumkin, lekin raw pointer dereference yoki pointerdan reference yasash unsafe bo'lib qoladi. Demak `unsafe fn` yozish ham "endi hamma narsa ruxsat" degani emas.

### Safe Abstraction

```rust
fn safe_wrapper(input: &[i32]) -> i32 {
    assert!(!input.is_empty());
    // SAFETY: empty bo'lmasligi tekshirildi.
    unsafe { *input.as_ptr() }
}
```

Funksiya o'zi `unsafe` emas — chunki invariant ichkarida tekshirilgan.

## SAFETY Konventsiyasi

```rust
/// SAFETY: Caller must guarantee single-threaded access.
unsafe fn add_to_count(inc: u32) {
    unsafe {
        // SAFETY: only main thread.
        COUNTER += inc;
    }
}
```

Har `unsafe fn` tepasida va har `unsafe` blok yonida `SAFETY:` izohi — saqlash kerak bo'lgan invariantlarni hujjatlaydi.

## Common Mistakes

- **`unsafe` "tekshiruvlarni o'chiradi" deb o'ylash.** Borrow checker, type checker — hammasi davom etadi.
- **Unsafe blokni keng qilish.** Memory bug topilganda barcha joyga qarashga to'g'ri keladi.
- **`SAFETY:` izohini yozmaslik.** Kod auditi imkonsiz bo'ladi.
- **Public API `unsafe` qilib chiqarish.** Mumkin bo'lganda safe abstraction yarating.
- **`unsafe fn` body'ni avtomatik to'liq unsafe deb qabul qilish.** Ayniqsa Rust 2024 semanticsida explicit `unsafe {}` scope afzal.

## Toolchain

- **Statik:** kompilyator + lint'lar
- **Runtime:** [[miri|Miri]] — UB detection
- **Sanitizers:** AddressSanitizer, ThreadSanitizer (nightly)

## Related Concepts

- [[unsafe-superpowers|unsafe superpowers]] — 5 ta operatsiya batafsil
- [[memory-safety|memory safety]] — `unsafe` chetlab o'tishi mumkin
- [[safe-abstraction|safe abstraction]] — eng muhim pattern
- [[raw-pointer|raw pointer]]
- [[raw-borrow-operators|raw borrow operators]]
- [[mutable-static|mutable static]]
- [[unsafe-trait|unsafe trait]]
- [[union-type|union]]
- [[undefined-behavior|undefined behavior]]
- [[ffi|FFI]]
- [[extern-block|extern block]]
- [[miri|Miri]]
- [[ownership]]
- [[compiler]]

## Sources

- [[wiki/sources/0-2-introduction|0.2 Introduction]]
- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-safe-rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
