---
title: "E0369 binary operation cannot be applied"
type: error
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, compiler-error, generics, traits]
source_count: 1
---

# E0369 binary operation cannot be applied

## Symptom

Compiler `error[E0369]: binary operation '>' cannot be applied to type '&T'` kabi diagnostic beradi.

Chapter 10.1dagi generic `largest<T>` functionida xato `if item > largest` line'ida paydo bo'ladi, chunki `T` har qanday type bo'lishi mumkin.

## Cause

Generic type parameter body ichidagi operationlarni avtomatik support qilmaydi. `>` ishlatish uchun values comparison behavioriga ega bo'lishi kerak. Rust bu behaviorni [[partial-ord|PartialOrd]] trait orqali ifodalaydi.

`fn largest<T>(list: &[T]) -> &T` signature'i `T`ni cheklamaydi, shuning uchun compiler `&T > &T` har doim ishlaydi deb ishonolmaydi.

## Fix Pattern

Type parameterga trait bound qo'shing:

```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Bu signature `largest` faqat `PartialOrd` implement qilgan typelar bilan chaqirilishini bildiradi.

## Minimal Example

Failing:

```rust
fn largest<T>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Fixed:

```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

## Related Concepts

- [[10-1-generic-data-types]]
- [[generic-functions|generic functions]]
- [[generic-type-parameters|generic type parameters]]
- [[traits]]
- [[partial-ord|PartialOrd]]
- [[generics]]
