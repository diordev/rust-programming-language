---
title: "Rust for Backend Developers: Iterators"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, iterators, chapter]
source_count: 1
---

# Rust for Backend Developers: Iterators

## Learning Goal

Iterator modelini `for` syntaxidan ajratib tushunish: `Iterator`, `IntoIterator`, `.iter()`/`.into_iter()`, `Range`, lazy adapters, va consuming operationsni ownership bilan bir chiziqda ko'rish.

## Main Ideas

- `for` loop collection ustida emas, iterator ustida ishlaydi; collection bo'lsa, u `IntoIterator` orqali iteratorga aylantiriladi.
- `.iter()` borrowed itemlar beradi, `.into_iter()` esa owned itemlar berib collectionni consume qilishi mumkin.
- `for x in collection` va `for x in &collection` bir xil ko'rinsa ham, ownership natijasi boshqa.
- `Range` iteratorning foydali istisnosi: u alohida iterator wrapper emas, o'zi `Iterator`.
- `map`, `filter`, `filter_map` kabi iterator adapters lazy; `collect`, `sum`, `fold`, `reduce`, `find` esa computationni haqiqatan ishga tushiradi.
- `fold` tashqi accumulator bilan ishlaydi, `reduce` esa first item'dan boshlagani uchun `Option` qaytaradi.
- `filter_map` va `find` iterator modelini `Option` bilan bevosita bog'laydi.

## Concepts

- [[iterators]]
- [[into-iterator|IntoIterator]]
- [[iterator-adapters|iterator adapters]]
- [[consuming-adapters|consuming adapters]]
- [[collect]]
- [[range]]
- [[for-loop|for loop]]
- [[option|Option]]
- [[closures]]
- [[fn-traits|Fn traits]]

## Examples

```rust
let v = vec![1, 2, 3];

for x in &v {
    println!("{x}");
}
```

```rust
let squares = vec![1, 2, 3, 4]
    .into_iter()
    .filter(|x| x % 2 == 0)
    .map(|x| x * x)
    .collect::<Vec<_>>();
```

```rust
let total = [1, 2, 3].into_iter().fold(0, |acc, x| acc + x);
let first_even = [1, 3, 6, 7].into_iter().find(|x| x % 2 == 0);
```

## Exercises

- `.iter()` va `.into_iter()` bilan bir xil collection ustida item type va ownership farqini yozing.
- Custom iterator yozing va `next()`ni qo'lda chaqirib state qanday siljishini ko'rsating.
- `filter_map` bilan `map + flatten`ni bir xil pipeline'da solishtiring.
- `fold` va `reduce`ni bo'sh collection case bilan birga solishtiring.

## Review Questions

- `for` loop nega `IntoIterator` bilan bog'langan, `Iterator` bilan emas?
- `.iter()` va `.into_iter()` orasidagi ownership farqi nima?
- Iterator laziness qayerda tugaydi?
- `fold` qachon `reduce`dan kuchliroq?
- `find` nega `Option` qaytaradi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-iterators]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[iterators]]
- [[into-iterator|IntoIterator]]
- [[iterator-adapters|iterator adapters]]
- [[consuming-adapters|consuming adapters]]
- [[collect]]
- [[range]]
- [[for-loop|for loop]]
