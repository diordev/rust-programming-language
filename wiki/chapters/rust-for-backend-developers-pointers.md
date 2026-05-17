---
title: "Rust for Backend Developers: Pointers"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, pointers, chapter]
source_count: 1
---

# Rust for Backend Developers: Pointers

## Learning Goal

Raw pointerlar, `unsafe`, dereference, va borrow checker'dan tashqariga chiqish narxini tushunish.

## Main Ideas

- `*const T` va `*mut T` raw pointer type'lari.
- Pointer yaratish safe bo'lishi mumkin, dereference esa unsafe.
- `&raw const` va `&raw mut` zamonaviy raw borrow operatorlari.
- Raw pointerlar borrow checker cheklovlarini chetlab o'tadi, lekin invariantlar endi dasturchi zimmasida.
- Unsafe kod Miri va test bilan qattiq tekshirilishi kerak.

## Concepts

- [[raw-pointer]]
- [[raw-borrow-operators|raw borrow operators]]
- [[dereference-operator|dereference operator]]
- [[unsafe-rust|unsafe Rust]]
- [[miri|Miri]]

## Examples

```rust
let ptr: *const i32 = &raw const value;
unsafe {
    println!("{}", *ptr);
}
```

## Exercises

- Bitta qiymat uchun cast va `&raw` bilan pointer oling.
- Raw pointer dereference qaysi invariantlarga tayanishini yozing.

## Review Questions

- Reference va raw pointer o'rtasidagi asosiy safety farqi nima?
- `unsafe fn` ichida ham nima uchun explicit unsafe scope yaxshi odat?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-pointers]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[raw-pointer]]
- [[unsafe-rust|unsafe Rust]]
