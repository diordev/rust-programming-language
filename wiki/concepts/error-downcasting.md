---
title: "Error Downcasting"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, error-handling, trait-objects]
source_count: 1
---

# Error Downcasting

## Short Definition

Erased error (`dyn Error` yoki `anyhow::Error`) ichidan ma'lum concrete error type'ni `downcast_ref` bilan ajratib olish.

## Why It Matters

Ko'pincha app-level boundary'da erased error qaytariladi, lekin ba'zan bir-ikki specific type uchun maxsus branch kerak bo'ladi.

## Mental Model

Type erasure'dan keyin "haqiqiy type shu bo'lsa" deb tekshirasiz.

## Syntax and Examples

```rust
if let Some(err_a) = e.downcast_ref::<ErrA>() {
    println!("Handle ErrA separately: {err_a}");
}
```

## Common Mistakes

- Downcasting'ni default design qilish.
- Erased error qaytarib, keyin ko'p joyda concrete typelarni qayta qidirish.

## Related Concepts

- [[box-dyn-error|Box<dyn Error>]]
- [[root-cause]]
- [[downcasting]]

## Sources

- [[wiki/sources/rust-for-backend-developers-error-handling]]
