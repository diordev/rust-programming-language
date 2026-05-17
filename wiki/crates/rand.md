---
title: "rand"
type: crate
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, crate, random]
source_count: 2
---

# rand

## Short Definition

`rand` random number generation uchun Rust ecosystemdagi external crate. Chapter 2 guessing game projectida secret number yaratish uchun ishlatiladi.

## Cargo Dependency

```toml
[dependencies]
rand = "0.8.5"
```

## Basic Usage

```rust
use rand::Rng;

let secret_number = rand::thread_rng().gen_range(1..=100);
```

## Notes

- `rand` standard library ichida emas, external crate sifatida `Cargo.toml`ga qo'shiladi.
- `Rng` trait scope ichida bo'lishi kerak, chunki `gen_range` method shu trait orqali keladi.
- External crate item'lari pathda crate nomidan boshlanadi, masalan `rand::thread_rng()`.
- `1..=100` inclusive range expression, ya'ni 1 va 100 ham mumkin.
- `cargo doc --open` dependency documentationni local browserda ochib beradi.

## Related Pages

- [[wiki/sources/2-programming-a-guessing-game]]
- [[guessing-game]]
- [[cargo|Cargo]]
- [[crate]]
- [[dependencies]]
- [[use-declarations|use declarations]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
