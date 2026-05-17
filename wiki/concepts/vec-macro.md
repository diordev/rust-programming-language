---
title: "vec! macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, collections, macros]
source_count: 4
---

# vec! macro

## Short Definition

`vec!` macro initial valuesdan yangi [[vector|Vec<T>]] yaratadi.

## Why It Matters

`vec!` boilerplateni kamaytiradi va compilerga element type'ini initial valuesdan infer qilishga yordam beradi.

## Mental Model

`vec![1, 2, 3]` "shu elementlar bilan vector yarat" degani. Elementlar bir xil concrete type'ga mos bo'lishi kerak.

20.5 bo'limida `vec!` [[declarative-macros|declarative macro]] sifatida ochiladi: u expressionlar ro'yxatini pattern bilan capture qilib, `Vec::new()` va `push` chaqiriqlariga expand bo'ladi.

Beginner backend source bu macro'ni amaliy ergonomics nuqtasidan kiritadi: bir nechta `push` o'rniga odatda birinchi tanlov `vec![...]`.

Declarative macro chapter bu tanlovning til-dizayn sababini ham ochadi: Rust functions variable number of arguments qabul qilmaydi, macro esa `$(...),*` repetition bilan qabul qila oladi.

## Syntax and Examples

```rust
let v = vec![1, 2, 3];
```

Bu odatda `Vec<i32>` sifatida inferred bo'ladi, chunki integer default type'i `i32`.

`vec!(1, 2, 3)` yoki `vec!{1, 2, 3}` ham syntax jihatdan macro invocation sifatida ishlashi mumkin, lekin `[]` eng odatiy ko'rinish.

Soddalashtirilgan macro definition: [[simplified-vec-macro]].

## Common Mistakes

- `vec!`ni type emas deb unutish; u macro, natijasi `Vec<T>`.
- Bir `vec![]` ichiga mos kelmaydigan har xil typelarni joylashtirish.

## Related Concepts

- [[vector|Vec<T>]]
- [[macro]]
- [[declarative-macros|declarative macros]]
- [[simplified-vec-macro]]
- [[type-inference|type inference]]
- [[collections]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors]]
- [[wiki/sources/20-5-macros|20.5 Macros]]
- [[wiki/sources/rust-for-backend-developers-vector]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
