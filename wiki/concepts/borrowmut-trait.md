---
title: "BorrowMut Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, traits, borrowing, mutable]
source_count: 1
---

# BorrowMut Trait

## Short Definition

`BorrowMut<T>` trait'i owner type'dan `&mut T` mutable borrowed view beradi.

## Why It Matters

Bu `Borrow`ning mutable analogi bo'lib, semantic borrowing contract bilan birga mutable access kerak bo'lgan hollarda ishlatiladi.

## Mental Model

`BorrowMut` tashqi ko'rinishda `AsMut`ga o'xshaydi, lekin `Borrow` oilasiga tegishli bo'lgani uchun u faqat convenient reference conversion emas, balki stronger borrowed view modelining bir qismi sifatida o'qiladi.

## Syntax and Examples

```rust
pub trait BorrowMut<Borrowed>: Borrow<Borrowed>
where
    Borrowed: ?Sized,
{
    fn borrow_mut(&mut self) -> &mut Borrowed;
}
```

## Common Mistakes

- `BorrowMut` va `AsMut`ni semantic jihatdan farqsiz deb o'ylash.
- Mutable borrowed view collection consistency contractlaridan mustaqil deb qarash.

## Related Concepts

- [[borrow-trait]]
- [[asmut-trait]]
- [[mutable-reference|mutable reference]]

## Sources

- [[wiki/sources/rust-for-backend-developers-common-traits]]

