---
title: "Слайсы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, slices]
source_count: 1
---

# Слайсы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/15. Слайсы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/slices.html

## Detailed Summary

Bu source slice'ni sequence ustidagi special borrowed view sifatida tushuntiradi. Oddiy reference bitta value'ga ishora qilsa, slice odatda array, vector yoki boshqa contiguous sequence'ning bir qismiga qaraydi.

Syntax `&arr[..]`, `&arr[2..=4]`, `&arr[2..5]` misollari bilan beriladi. Muhim detail: indexing slice boshlanishiga nisbatan ishlaydi, original collection boshlanishiga emas.

Source slice memory modelini ham ochadi: slice value pointer + length juftligi sifatida tasavvur qilinadi. Bu oddiy `&T`dan bir qadam boyroq mental model. Shu sabab slice ko'pincha "fat pointer" sifatida tushuntiriladi.

Mutable slice ham ko'rsatiladi: `&mut v[1..3]`. Bu borrowed subrange orqali original vectorning ma'lum qismini o'zgartirish imkonini beradi. Shu bilan slice borrowing, mutability, va contiguous memory modelini bir nuqtada birlashtiradi.

## Key Concepts

- [[slices]]
- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[array]]
- [[vector|Vec<T>]]

## Code Examples

```rust
let arr: [i32; 5] = [0, 1, 2, 3, 4];
let slice1: &[i32] = &arr[..];
let slice2: &[i32] = &arr[2..=4];
```

```rust
let arr = [0, 1, 2, 3, 4];
let slice: &[i32] = &arr[2..=4];
println!("{}", slice.len());
println!("{}", slice[2]);
```

```rust
let mut v: Vec<i32> = vec![0, 1, 2, 3];
let slice: &mut [i32] = &mut v[1..3];
slice[0] = 9;
```

## Exercises or Practice Ideas

- Bir functionni `Vec<T>` qabul qilishdan `&[T]` qabul qilishga refactor qiling.
- Inclusive va exclusive range bilan olingan slice misollarini yozing.
- Mutable slice orqali vectorning kichik qismi o'zgarganda original vector holatini kuzating.

## Questions Raised

- `&[T]` parameter qachon `&Vec<T>`dan yaxshiroq abstraction?
- Slice pointer + length modeli borrowing qoidalari bilan qanday kesishadi?
- String slices va collection slices orasidagi umumiylik qanchalik katta?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-slices]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[slices]]
- [[reference]]
- [[borrowing]]
