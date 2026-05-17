---
title: "Generic Functions"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, generics, functions]
source_count: 2
---

# Generic Functions

## Short Definition

Generic function concrete parameter yoki return type o'rniga generic type parameter ishlatadigan function. Rustda type parameter function nomidan keyin `<>` ichida e'lon qilinadi.

## Why It Matters

Generic functions bir xil body'ni `i32`, `char`, `String`, yoki boshqa concrete typelar uchun qayta yozmaslikka yordam beradi. Bu [[code-duplication|code duplication]]ni kamaytiradi va function contractni type darajasida umumlashtiradi.

## Mental Model

`fn largest<T>(list: &[T]) -> &T` "largest functioni qandaydir `T` type ustida generic" deb o'qiladi. `list` `T` elementlar slice'i, return value esa shu slice ichidagi `T` elementga reference.

Generic signature body'ga har qanday operationni bermaydi. Body `>` ishlatsa, `T` comparable bo'lishi kerak. Bunday behavior [[traits|trait]] bound bilan ko'rsatiladi, masalan `T: std::cmp::PartialOrd`.

Chapter 10.2 generic function parameters uchun [[impl-trait|impl Trait]] va [[trait-bounds|trait bounds]]ni ko'rsatadi. `&impl Summary` simple case uchun concise, `T: Summary` esa two parameters same concrete type bo'lishi kerak kabi relationshipsni ifodalaydi.

## Syntax and Examples

```rust
fn identity<T>(value: T) -> T {
    value
}
```

`largest<T>` shape:

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

Trait as parameter:

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

Trait bound equivalent:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

## Common Mistakes

- `fn largest(list: &[T]) -> &T` deb yozib, `T`ni `largest<T>`da e'lon qilmaslik.
- Generic function body har qanday `T` uchun ishlaydi deb o'ylash.
- `>` kabi operationlar uchun kerakli [[partial-ord|PartialOrd]] yoki boshqa trait boundni unutish.
- Generic function return type'idagi `T` inputdagi `T` bilan bir xil concrete typega resolve bo'lishini unutish.
- `&impl Summary` ikki parameter uchun turli concrete typelarni ruxsat qilishini unutish.
- Same-type relationship kerak bo'lganda `T: Summary` ishlatmaslik.

## Related Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[functions]]
- [[traits]]
- [[trait-bounds|trait bounds]]
- [[impl-trait|impl Trait]]
- [[where-clauses|where clauses]]
- [[partial-ord|PartialOrd]]
- [[generic-largest-function|generic largest function]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]

## Sources

- [[10-1-generic-data-types]]
- [[10-2-defining-shared-behavior-with-traits]]
