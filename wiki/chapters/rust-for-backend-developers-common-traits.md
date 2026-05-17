---
title: "Rust for Backend Developers: Common Traits"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, traits, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Common Traits

## Learning Goal

Rust standard/common traitlarini semantic contract sifatida o'qish: equality, ordering, conversion, borrowing, ownership cleanup, dynamic sized types, va thread-safety markerlari qaysi promise'larni berishini ajratish.

## Main Ideas

- `PartialEq` va `Eq` bir xil emas; `Eq` reflexive total equality signalidir, float `NaN` esa shu contract'ni buzadi.
- `PartialOrd` `Option<Ordering>` qaytaradi, `Ord` esa `Ordering`; demak hamma ordering comparable emas.
- `From`ni implement qilish conversionning canonical yo'li; `Into` ko'pincha undan avtomatik chiqadi.
- `AsRef` reference conversion beradi, `Borrow` esa bundan tashqari `Eq`/`Ord`/`Hash` consistency contractini talab qiladi.
- `ToOwned` borrowed view'dan owned value beradi; u ko'p holatda clone semanticsiga yaqin, lekin boshqa type ham qaytarishi mumkin.
- `Drop` cleanup hook, `Sized` esa compile-time size contracti; `Send` va `Sync` esa concurrency safety markerlari.

## Concepts

- [[eq-trait]]
- [[partial-eq]]
- [[partial-ord]]
- [[ord-trait|Ord]]
- [[ordering|Ordering]]
- [[default-trait]]
- [[from-trait]]
- [[into-trait]]
- [[asref-trait]]
- [[asmut-trait]]
- [[borrow-trait]]
- [[borrowmut-trait]]
- [[to-owned-trait]]
- [[drop]]
- [[sized-trait]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]

## Examples

```rust
let a = f32::NAN;
assert!(a != a);
```

```rust
impl PartialOrd for Cargo {
    fn partial_cmp(&self, other: &Self) -> Option<std::cmp::Ordering> {
        Some(self.cmp(other))
    }
}
```

```rust
let slice: &str = "aaa";
let owned: String = slice.to_owned();
```

## Exercises

- `Version` yoki `Priority` type'i uchun `Eq`/`Ord` derive qilib, domain semantics mos kelishini tekshiring.
- `AsRef` va `Borrow` qabul qiladigan ikki function yozib, contract farqini comment bilan tushuntiring.
- `str`, `&str`, `dyn Trait`, `&dyn Trait`, `Box<dyn Trait>`ni `Sized` nuqtai nazaridan jadvalga tushiring.

## Review Questions

- Nega `Eq` floatlarga to'g'ri kelmaydi?
- `PartialOrd`ning `Option<Ordering>` qaytarishi nimani signal qiladi?
- `From` implement qilinganda `Into` bilan nima bo'ladi?
- `Borrow` nega oddiy `AsRef` emas?
- `?Sized` bound qachon kerak bo'ladi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-common-traits]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-traits]]

