---
title: "Кортежи - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, tuples]
source_count: 1
---

# Кортежи - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/20. Кортежи.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/tuples.html

## Detailed Summary

Bu source tuple'ni fixed-size, lekin heterogeneous group sifatida tanishtiradi. Array bilan boundary aniq: array elementlari bir xil type bo'lishi kerak, tuple esa turli typelarni bitta value ichiga yig'a oladi.

Access positional: `.0`, `.1`, `.2`. Shu bilan birga destructuring syntax bilan tuple elementlarini birdaniga alohida variables'ga ajratish mumkin.

Source tuple'ning eng foydali amaliy ishlatishlaridan birini ko'rsatadi: functiondan bir nechta qiymat qaytarish. `split_to_odd_and_even(numbers: &[i32]) -> (Vec<i32>, Vec<i32>)` misoli tuple'ni slices, vectors, loops, va if bilan bir nuqtada birlashtiradi.

Muhim qo'shimcha insight: function parameter sifatida `&[i32]` ishlatilgani sabab bir xil functionni vector va array uchun ishlatish mumkin. Tuple chapter faqat tuples emas, balki slice-based API design'ni ham tabiiy ravishda mustahkamlaydi.

## Key Concepts

- [[tuple]]
- [[slices]]
- [[vector|Vec<T>]]
- [[for-loop|for loop]]
- [[if-expressions|if expressions]]
- [[functions]]

## Code Examples

```rust
let employee: (&str, i32, bool) = ("John Doe", 1980, true);
println!("{}", employee.0);
let (name, birth_year, is_active) = employee;
```

```rust
fn split_to_odd_and_even(numbers: &[i32]) -> (Vec<i32>, Vec<i32>) {
    let mut odds = Vec::new();
    let mut evens = Vec::new();
    for n in numbers {
        if n % 2 != 0 {
            odds.push(*n);
        } else {
            evens.push(*n);
        }
    }
    (odds, evens)
}
```

## Exercises or Practice Ideas

- Tuple va struct bilan bir xil domain objectni alohida modellashtirib solishtiring.
- Functiondan ikki qiymat qaytaradigan minimal example yozing.
- Bitta functionni `&[i32]` parameter bilan array ham, vector ham qabul qiladigan qiling.

## Questions Raised

- Qaysi nuqtadan boshlab tuple o'rniga named struct ishlatish kerak?
- Tuple return value readability'ni qachon pasaytiradi?
- `&[T]` parameter bilan generic collection API mental modeli qay darajada erta berilishi kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-tuples]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[tuple]]
- [[slices]]
- [[functions]]
