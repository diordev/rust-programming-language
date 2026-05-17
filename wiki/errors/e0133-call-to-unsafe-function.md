---
title: "E0133 call to unsafe function"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, error, unsafe]
source_count: 1
---

# E0133 — Call to Unsafe Function

## Symptom

```text
error[E0133]: call to unsafe function `dangerous` is unsafe and requires
              unsafe block
 --> src/main.rs:4:5
  |
4 |     dangerous();
  |     ^^^^^^^^^^^ call to unsafe function
```

## Cause

`unsafe fn` bilan belgilangan funksiyani **`unsafe { ... }` bloki tashqarisida** chaqirish.

Unsafe funksiyalar ma'lum invariantlarga ega bo'lib, kompilyator ularni tekshira olmaydi. Caller ushbu invariantlarni saqlashga `unsafe` blok orqali va'da beradi.

## Fix Pattern

### 1. `unsafe` blok bilan o'rab chaqirish

```rust
unsafe fn dangerous() {}

fn main() {
    unsafe {
        dangerous();
    }
}
```

### 2. Caller funksiyani ham `unsafe` qilish

Agar siz boshqa unsafe funksiya yozmoqchi bo'lsangiz va shu chaqiruv unsafe API'ngizning qismi bo'lsa:

```rust
unsafe fn outer() {
    unsafe {
        dangerous();    // hali ham unsafe block kerak (Rust 2024)
    }
}
```

### 3. Safe abstraction bilan o'rab olish

Agar invariantlarni dasturchi tomonidan saqlash mumkin bo'lsa, unsafe funksiyani safe API ichida bekitamiz:

```rust
unsafe fn raw_op(...) { /* ... */ }

pub fn safe_wrapper(input: ...) -> ... {
    // invariantlarni tekshirish
    assert!(input_is_valid(&input));
    // SAFETY: input validatsiyalangan, raw_op contracti bajariladi.
    unsafe { raw_op(...) }
}
```

## Minimal Example

```rust
fn main() {
    unsafe fn dangerous() {}

    dangerous();   // E0133
}
```

To'g'rilash:

```rust
fn main() {
    unsafe fn dangerous() {}

    unsafe { dangerous() }
}
```

## Related

- [[unsafe-rust|unsafe Rust]]
- [[unsafe-superpowers|unsafe superpowers]] — superpower #2
- [[safe-abstraction|safe abstraction]] — unsafe funksiyani safe API ichiga o'rash
- [[function-contracts|function contracts]] — `SAFETY:` izohi konventsiyasi
- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
