---
title: "PartialOrd"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits, comparisons]
source_count: 2
---

# PartialOrd

## Short Definition

`PartialOrd` standard library trait'i qiymatlarni partial ordering bilan solishtirish imkonini bildiradi; `>`, `<`, `>=`, va `<=` kabi comparison operationlar shu behaviorga tayanadi.

## Why It Matters

Generic code comparison ishlatsa, compiler type parameter hamma possible type uchun comparable ekanini bilishi kerak. `largest<T>` misolida `item > largest` line'i uchun `T: std::cmp::PartialOrd` constraint kerak.

## Mental Model

`T` degani "istalgan type" emas, "signature ruxsat bergan type". Agar function body `>` ishlatsa, `T` kamida `PartialOrd` implement qilgan type bo'lishi kerak. `i32` va `char` buni implement qiladi, shuning uchun `largest` ular bilan ishlaydi.

Chapter 10.2da `Pair<T>` example'i `PartialOrd`ni [[display-formatting|Display]] bilan birga conditional method bound sifatida ishlatadi: `cmp_display` methodi `>=` comparison uchun `PartialOrd`, `{}` output uchun `Display` talab qiladi.

## Syntax and Examples

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

Conditional method with `PartialOrd`:

```rust
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

## Common Mistakes

- `T` har qanday operationni support qiladi deb o'ylash.
- Compiler errorida `PartialOrd` taklifini ko'rib, bu generic syntax emas, behavior constraint ekanini sezmaslik.
- `PartialOrd` va boshqa comparison/order traits orasidagi farqni keyinroq aniqlashtirmaslik.

## Related Concepts

- [[traits]]
- [[generics]]
- [[generic-functions|generic functions]]
- [[generic-type-parameters|generic type parameters]]
- [[trait-bounds|trait bounds]]
- [[conditional-method-implementations|conditional method implementations]]
- [[display-formatting|Display]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]

## Sources

- [[10-1-generic-data-types-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
