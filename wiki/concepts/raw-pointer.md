---
title: "Raw Pointer"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, unsafe, pointers, memory]
source_count: 2
---

# Raw Pointer

## Short Definition

`*const T` (immutable) yoki `*mut T` (mutable) — Rust borrow qoidalariga bo'ysunmaydigan low-level pointerlar. Reference (`&T`, `&mut T`) bilan o'xshash, lekin xavfsizlik kafolatlarisiz.

## Why It Matters

Rust borrow checker ko'p ish qiladi, lekin ba'zan kompilyator bilmaydigan ma'lumot dasturchida bo'ladi: FFI orqali C kodi bilan ishlash, donadek smart pointer yozish, yoki performance-critical strukturalar. Raw pointer "kompilyator, ishonib qo'y" deyishning yo'li.

## Mental Model

Raw pointer — oddiy adres. Borrow checker uni kuzatmaydi. Dereference qilish (`*ptr`) faqat `unsafe` blok ichida ruxsat etiladi, chunki bu xato bo'lishi (UB) mumkin.

Rustda raw pointer yaratishning uch amaliy yo'li ko'p uchraydi: reference'dan cast, `&raw const` / `&raw mut`, va eski `addr_of!` / `addr_of_mut!`.

| Xususiyat | `&T` / `&mut T` | `*const T` / `*mut T` |
|-----------|------------------|------------------------|
| Borrow rules | Majburiy | Yo'q |
| Valid memoryga ko'rsatish | Kafolatlangan | Kafolatlanmagan |
| `null` bo'lishi | Mumkin emas | Mumkin |
| Avtomatik cleanup | Yo'q (lifetime) | Yo'q |
| Bir vaqtda mutable + immutable | Mumkin emas | Mumkin (data race xavfi) |

## Syntax and Examples

### Yaratish — yangi raw borrow operatorlari (Rust 2024)

```rust
let mut num = 5;

let r1 = &raw const num;   // *const i32
let r2 = &raw mut num;     // *mut i32
```

`&raw const` va `&raw mut` — yangi sintaksis (Rust 2024 da prelude). Eski `&num as *const _` o'rniga ishlatiladi.

### Ixtiyoriy adresga raw pointer (xavfli)

```rust
let address = 0x012345usize;
let r = address as *const i32;   // segfault yoki UB xavfi
```

### Dereference — faqat `unsafe` blok ichida

```rust
let mut num = 5;
let r1 = &raw const num;
let r2 = &raw mut num;

unsafe {
    println!("r1 = {}", *r1);
    *r2 = 10;
    println!("r2 = {}", *r2);
}
```

Eski helper'lar:

```rust
let r3 = std::ptr::addr_of!(num);
let r4 = std::ptr::addr_of_mut!(num);
```

Yaratish xavfsiz, lekin **dereference** xavfli — chunki adresni dereference qilganda haqiqiy xotiraga tegamiz.

## Common Mistakes

- **Ixtiyoriy adresni cast qilish.** `0x1234 as *const i32` — adresda nima borligi noma'lum, dereference UB.
- **`*` ni dereference operator deb hisoblash.** `*const T` ichidagi `*` — type sintaksisi qismi, dereference emas. Dereference esa boshqa `*` (`*ptr`).
- **Bir vaqtda mutable va immutable raw pointer orqali yozib o'qish.** Bu data race; reference o'rnidagi qoida raw pointerda yo'q, lekin UB shart.
- **Raw pointerni boshqa thread'ga uzatish.** `Send` emas, lekin `unsafe impl Send` bilan majburlash mumkin — UB xavfini o'zingiz olasiz.

## Related Concepts

- [[unsafe-rust|unsafe Rust]] — raw pointer dereference faqat `unsafe` ichida
- [[raw-borrow-operators|raw borrow operators]] — `&raw const`, `&raw mut`
- [[reference]] — raw pointerga safe alternativa
- [[mutable-reference|mutable reference]] — borrow checker bilan
- [[dangling-reference|dangling reference]] — raw pointerda osonroq sodir bo'ladi
- [[data-race|data race]] — mutable raw pointer bilan
- [[ffi|FFI]] — C bilan ishlashda zarur
- [[undefined-behavior|undefined behavior]] — raw pointer xato ishlatilganda
- [[smart-pointers]] — safe abstraction qatlami
- [[miri|Miri]]

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
