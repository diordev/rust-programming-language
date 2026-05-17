---
title: "clone"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, ownership]
source_count: 3
---

# clone

## Short Definition

`clone()` value'ning explicit copy'sini yaratadigan method. Heap-backed typelarda bu deep copy bo'lishi va costly bo'lishi mumkin.

## Why It Matters

Rust automatic deep copy qilmaydi. Agar `String` kabi value'ning mustaqil copy'si kerak bo'lsa, `clone()` bilan intent va possible cost aniq ko'rsatiladi.

## Mental Model

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {s1}, s2 = {s2}");
```

Bu yerda `s1` ham, `s2` ham valid qoladi.

`Clone` faqat convenient method emas; u ecosystemdagi ko'p generic API'lar uchun shared contract hamdir. Shuning uchun custom helper method o'rniga `Clone` trait'idan foydalanish odatda to'g'riroq.

Appendix C nuqtai nazaridan, `#[derive(Clone)]` type'ning har bir qismini `clone()` qilib, butun value'ni reconstruct qiladi. Shu sabab barcha fieldlar ham `Clone` bo'lishi kerak.

## Syntax and Examples

```rust
let copied_heap_data = original.clone();
```

Derived clone:

```rust
#[derive(Clone)]
struct Buffer {
    bytes: Vec<u8>,
}
```

Qo'lda implementation ham mumkin:

```rust
impl Clone for Buffer {
    fn clone(&self) -> Self {
        Self {
            bytes: self.bytes.clone(),
        }
    }
}
```

## Common Mistakes

- Move kerak bo'lgan joyda odat bo'yicha `clone()` ishlatish.
- `clone()`ni free operation deb o'ylash.
- `Copy` typelarda `clone()`ni ownership mental modelini tushunmasdan ishlatish.
- `derive(Clone)` har qanday field kombinatsiyasi uchun ishlaydi deb o'ylash.
- `Clone` bound ergonomika beradi, lekin hidden cost yo'q deb o'ylash.

## Related Concepts

- [[move-vs-clone]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[string-type|String]]
- [[derive-attribute]]
- [[marker-trait|marker trait]]

## Sources

- [[4-1-what-is-ownership]]
- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
- [[wiki/sources/22-3-c-derivable-traits|22.3]]
