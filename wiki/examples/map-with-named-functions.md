---
title: "map with Named Functions and Enum Initializers"
type: example
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, iterators, functions, enums]
source_count: 1
---

# map with Named Functions and Enum Initializers

## Context

Rust Book Listings 20-29, 20-30, va 20-31 `Iterator::map` closure qabul qilsa ham named function va enum variant initializer functionlar bilan ishlashini ko'rsatadi.

## Code

Closure bilan:

```rust
fn main() {
    let list_of_numbers = vec![1, 2, 3];
    let list_of_strings: Vec<String> =
        list_of_numbers.iter().map(|i| i.to_string()).collect();
}
```

Named function bilan:

```rust
fn main() {
    let list_of_numbers = vec![1, 2, 3];
    let list_of_strings: Vec<String> =
        list_of_numbers.iter().map(ToString::to_string).collect();
}
```

Enum variant initializer bilan:

```rust
fn main() {
    enum Status {
        Value(u32),
        Stop,
    }

    let list_of_statuses: Vec<Status> = (0u32..20).map(Status::Value).collect();
}
```

## Explanation

`map` har elementni boshqa qiymatga aylantiruvchi callback kutadi. Callback closure bo'lishi mumkin:

```rust
|i| i.to_string()
```

Lekin named function ham `Fn` traitini implement qilgani uchun ishlaydi:

```rust
ToString::to_string
```

Bu yerda [[fully-qualified-syntax|fully qualified syntax]] ishlatiladi, chunki `to_string` nomli method bir nechta trait/type kontekstida bo'lishi mumkin.

Enum variantlar ham initializer function sifatida ishlaydi. `Status::Value`ning function-like signature'i:

```rust
fn(u32) -> Status
```

Shu sababli `(0u32..20).map(Status::Value)` har `u32`dan `Status::Value(u32)` hosil qiladi.

## Style Note

Quyidagi ikki yozuv ko'pincha bir xil compiled codega olib keladi:

```rust
.map(Status::Value)
.map(|n| Status::Value(n))
```

Tanlov readabilityga bog'liq. Initializer function qisqaroq; closure esa mapping logikasi murakkablashganda aniqroq bo'lishi mumkin.

## Related Concepts

- [[iterators]]
- [[iterator-adapters|iterator adapters]]
- [[closures]]
- [[function-pointers|function pointers]]
- [[fully-qualified-syntax|fully qualified syntax]]
- [[enum-variants|enum variants]]

## Sources

- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
