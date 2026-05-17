---
title: "20.1. Unsafe Rust"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, unsafe, ffi, raw-pointers]
source_count: 1
---

# 20.1. Unsafe Rust

## Learning Goal

[[unsafe-rust|Unsafe Rust]] — `unsafe` blok ichida ruxsat etilgan **5 ta superpower**'ni bilish, raw pointer'lar, FFI, mutable static, unsafe trait, va union'lar bilan to'g'ri ishlash. [[safe-abstraction|Safe abstraction]] pattern'ini o'zlashtirish va [[undefined-behavior|UB]] xavfini boshqarish.

## Main Ideas

- Rust ichida ikki til: **safe** (default) va **unsafe**.
- `unsafe` "kafolatlarni o'chirmaydi" — faqat 5 ta operatsiyaga ruxsat beradi.
- Borrow checker, type checker, va lifetime'lar `unsafe` blok ichida ham davom etadi.
- Eng yaxshi pattern: **safe abstraction over unsafe** — ichkarida `unsafe`, tashqarida safe API.
- Mutable global state (`static mut`) — deyarli har doim noto'g'ri tanlov; `Mutex`, `Atomic*` ishlatish kerak.
- FFI — Rust va boshqa tillar orasidagi ko'prik; `extern "C"` orqali.
- Miri — runtime'da UB topadi; static borrow checker'ni to'ldiradi.

## 5 ta Unsafe Superpower

| # | Superpower | Sahifa |
|---|------------|--------|
| 1 | Raw pointer dereference | [[raw-pointer]] |
| 2 | Unsafe function chaqirish | [[unsafe-superpowers]] |
| 3 | Mutable static'ga kirish | [[mutable-static]] |
| 4 | Unsafe trait implement | [[unsafe-trait]] |
| 5 | Union field'iga kirish | [[union-type]] |

## Yangi Sintaksis (Rust 2024)

### Raw borrow operatorlari

```rust
let mut num = 5;

let r1 = &raw const num;   // *const i32
let r2 = &raw mut num;     // *mut i32
```

[[raw-borrow-operators|Raw borrow operators]] eski `&num as *const _` cast'idan xavfsizroq — oraliq reference yaratmaydi.

### `safe` keyword extern blokda

```rust
unsafe extern "C" {
    safe fn abs(input: i32) -> i32;   // unsafe blok kerak emas
}

fn main() {
    println!("{}", abs(-3));
}
```

### `#[unsafe(no_mangle)]`

```rust
#[unsafe(no_mangle)]
pub extern "C" fn call_from_c() {
    println!("Hello from Rust!");
}
```

## Safe Abstraction — `split_at_mut`

```rust
use std::slice;

fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();

    assert!(mid <= len);

    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}
```

Borrow checker buni qabul qilmaydi (E0499), lekin invariant (`mid <= len`) saqlangani uchun safe abstraction. Funksiya o'zi `unsafe` emas → caller hech qanday majburiyat olmaydi.

Batafsil: [[split-at-mut-unsafe]].

## FFI Asoslari

```rust
unsafe extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("{}", abs(-3));
    }
}
```

`unsafe extern "C"` blokdagi har item implicitly unsafe. Rust kompilyatori boshqa til funksiyasi tanasini bilmaydi → tekshira olmaydi.

## Mutable Static — Xavf

```rust
static mut COUNTER: u32 = 0;

unsafe fn add_to_count(inc: u32) {
    unsafe { COUNTER += inc; }
}

fn main() {
    unsafe {
        add_to_count(3);
        println!("{}", *(&raw const COUNTER));
    }
}
```

- O'qish va yozish — ikkalasi unsafe.
- `&COUNTER` — `static_mut_refs` lint xato.
- Multi-thread'da — UB xavfi.
- **Tavsiya:** `Atomic*`, `Mutex<T>`, yoki `OnceLock<T>`.

## Unsafe Trait

```rust
unsafe trait Foo { /* methods */ }
unsafe impl Foo for i32 { /* impls */ }
```

Eng keng tarqalgan misol — [[send-trait|Send]] va [[sync-trait|Sync]] auto-trait'lar. Raw pointer'li struct uchun qo'lda `unsafe impl Send for ...` yozish ba'zan kerak.

## Miri — UB Detector

```shell
rustup +nightly component add miri
cargo +nightly miri test
```

Static borrow checker compile time'da, [[miri|Miri]] runtime'da ishlaydi. Listing 20-7 (ixtiyoriy adres):

```text
error: Undefined Behavior: pointer must be dereferenceable for 40000 bytes,
       but got 0x1234[noalloc] which is a dangling pointer
```

## SAFETY Konventsiyasi

```rust
/// SAFETY: Caller must guarantee single-threaded access; otherwise UB.
unsafe fn add_to_count(inc: u32) {
    unsafe {
        // SAFETY: This is only called from main thread (verified by caller).
        COUNTER += inc;
    }
}
```

Har `unsafe fn` va har `unsafe` blok yonida `SAFETY:` izoh — qaysi invariantlarni saqlash kerakligini hujjatlaydi.

## Concepts

- [[unsafe-rust|unsafe Rust]]
- [[unsafe-superpowers|unsafe superpowers]]
- [[raw-pointer|raw pointer]]
- [[raw-borrow-operators|raw borrow operators]]
- [[safe-abstraction|safe abstraction]]
- [[ffi|FFI]]
- [[extern-block|extern block]]
- [[mutable-static|mutable static]]
- [[unsafe-trait|unsafe trait]]
- [[union-type|union]]
- [[undefined-behavior|undefined behavior]]
- [[miri|Miri]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[memory-safety|memory safety]]
- [[data-race|data race]]

## Examples

- [[split-at-mut-unsafe|split_at_mut safe abstraction]] — Listing 20-4..20-7

## Errors

- [[e0133-call-to-unsafe-function|E0133: call to unsafe function]] — `unsafe { ... }` bloksiz chaqiruv
- [[e0499-multiple-mutable-borrows|E0499]] — `split_at_mut` nima sababdan safe Rust'da bo'lmasligi

## Exercises

1. **Slice utility:** `chunks_mut` ga o'xshash funksiya yozing — slice'ni teng `n` ta bo'laklarga ajratadi. Safe API, unsafe ichida.
2. **C interop:** `libc::strlen` ni Rust'dan chaqiring; null-terminated stringni Rust `&str` ga aylantiring.
3. **Atomic counter:** `static mut COUNTER` ni `AtomicU32` ga ko'chiring va benchmark farqini o'lchang.
4. **Miri experiment:** Listing 20-7 ni o'z muhitingizda Miri bilan ishlatib, xato matnini o'qing va sababini tahlil qiling.
5. **Unsafe trait:** Raw pointer'li `MyType { ptr: *mut u8 }` strukturasi yarating va uni `Send` qilish uchun nima saqlash kerakligini hujjatlang.
6. **Union vs enum:** Bitta `i32` ni 4 ta `u8` sifatida o'qish — `union` va `enum` bilan ikki yondashuv yozing va xavfsizligini taqqoslang.

## Review Questions

1. `unsafe` blok ichida qanday tekshiruvlar **hali ham** ishlaydi?
2. Raw pointer va reference orasida 4 ta asosiy farq qanday?
3. Nima uchun `split_at_mut` ni safe Rust'da yozib bo'lmaydi?
4. `unsafe extern "C"` va `extern "C"` ning farqi nima?
5. `safe` keyword extern blokda nima ifoda etadi?
6. `static mut` ga reference yaratish nega tavsiya etilmaydi?
7. Qaysi standart trait'lar `unsafe trait` bo'lib auto-derive qilinadi?
8. `union` va `enum` ning asosiy farqi nima?
9. Miri qaysi bug'larni topadi va qaysilarini topa olmaydi?
10. `SAFETY:` izoh konventsiyasining maqsadi nima?

## Related Pages

- [[wiki/sources/20-1-unsafe-rust|Source: 20.1]]
- [[wiki/chapters/20-advanced-features|Chapter 20]]
- [[wiki/chapters/16-fearless-concurrency|Chapter 16: Fearless Concurrency]] — `Send`/`Sync` kelib chiqishi
- [[wiki/chapters/15-smart-pointers|Chapter 15: Smart Pointers]] — safe abstraction'larning klassik misoli
