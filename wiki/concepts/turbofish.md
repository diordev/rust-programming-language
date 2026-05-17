---
title: "Turbofish"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-17
tags: [rust, generics, syntax]
source_count: 2
---

# Turbofish

## Short Definition

`::<...>` syntax'i expression context'da generic type, function, yoki method argumentlarini aniq ko'rsatish usuli.

## Why It Matters

Type inference yetarli bo'lmagan joyda compilerga qaysi concrete type kerakligini aytadi. Eng mashhur misol: `"42".parse::<i32>()`.

## Mental Model

`Vec<u8>` type context'dagi generic syntax bo'lsa, `parse::<i32>()` expression ichidagi generic specialization. `::` + `<...>` kombinatsiyasi shakli sabab community'da bu syntax "turbofish" deb ataladi.

Backend beginner source bu syntaxni compiler inference chegarasi bilan bog'laydi. `make_empty_vec<T>() -> Vec<T>` kabi zero-argument generic functionda inputdan `T`ni topib bo'lmaydi; yoki variable annotation yoziladi, yoki call site'da `make_empty_vec::<i32>()` ishlatiladi.

## Syntax and Examples

```rust
let n = "42".parse::<i32>().unwrap();
let bytes = Vec::<u8>::new();
```

Method, function, va type ustida ishlashi mumkin:

```rust
collect::<Vec<_>>()
Result::<u32, _>::Ok(5)
```

Zero-argument generic function:

```rust
fn make_empty_vec<T>() -> Vec<T> {
    Vec::new()
}

let xs: Vec<i32> = make_empty_vec();
let ys = make_empty_vec::<i32>();
```

## Common Mistakes

- `Vec<u8>` va `Vec::<u8>::new()` bir xil kontekst deb o'ylash.
- Type inference yetarli joylarda keraksiz turbofish bilan code'ni shovqinli qilish.
- `::<...>` o'rniga faqat `<...>` yozib parserni chalg'itish.
- Compiler hech qachon variable type annotation orqali generic resultni infer qila olmaydi deb o'ylash.

## Related Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[type-inference|type inference]]
- [[rust-operators-and-symbols]]
- [[functions]]
- [[methods]]
- [[type-annotations|type annotations]]

## Sources

- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
- [[wiki/sources/rust-for-backend-developers-generics]]
