---
title: "Generic Type Parameters"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, generics]
source_count: 3
---

# Generic Type Parameters

## Short Definition

Generic type parameters concrete type o'rnida ishlatiladigan placeholderlar. Rustda ular odatda `<T>`, `<K, V>`, yoki `<T, E>` shaklida yoziladi.

## Why It Matters

Generic type parameters bir xil logicni bir nechta concrete type bilan ishlatishga yordam beradi. Standard librarydagi `Option<T>`, `Vec<T>`, `HashMap<K, V>`, va `Result<T, E>` bunga misol.

## Mental Model

`T` "keyin concrete type bilan to'ldiriladigan type joyi" deb o'qiladi. Har concrete instantiation alohida type: `Option<i32>` va `Option<String>` bir xil emas.

Type parameter ishlatilishidan oldin e'lon qilinadi. Functionda `fn largest<T>(...)`, structda `struct Point<T>`, enumda `enum Option<T>`, method implementationda esa `impl<T> Point<T>` shakli ishlatiladi.

Rust convention bo'yicha generic type parameter nomlari qisqa va UpperCamelCase bo'ladi: `T`, `U`, `K`, `V`, `E`, yoki ko'proq aniqlik uchun `X1`, `Y1`, `X2`, `Y2`.

Trait bounds generic type parameterni behavior bilan cheklaydi. `T: Summary` `T` faqat `Summary` implement qilgan concrete type bo'lishini bildiradi; `T: Summary + Display` esa ikki capability talab qiladi.

## Syntax and Examples

```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Function:

```rust
fn largest<T>(list: &[T]) -> &T {
    &list[0]
}
```

Struct:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}
```

Impl block:

```rust
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
```

Trait bound:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Multiple bounds with `where`:

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
    unimplemented!()
}
```

Method-level generics:

```rust
impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}
```

## Common Mistakes

- `T` har qanday operationni support qiladi deb o'ylash.
- Generic parameterga kerakli behaviorni [[traits|trait]] bilan constrain qilish zarurligini unutish.
- `T`, `E`, `K`, `V` nomlari magic deb o'ylash; ular convention, lekin meaningni context beradi.
- `T`ni `fn`, `struct`, `enum`, yoki `impl` headerida e'lon qilmasdan ishlatish.
- Bitta `T` bir nechta fieldda ishlatilsa, o'sha fields bitta concrete type bo'lishi kerakligini unutish.
- Juda ko'p generic parameter readability va design signalini berishini e'tiborsiz qoldirish.
- Generic parameter bir nechta parameterda ishlatilsa, same concrete type relationship yaratilishini unutish.
- Trait boundni type annotation deb o'ylash; u behavior constraint.

## Related Concepts

- [[generics]]
- [[traits]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-enums|generic enums]]
- [[generic-methods|generic methods]]
- [[partial-ord|PartialOrd]]
- [[trait-bounds|trait bounds]]
- [[where-clauses|where clauses]]
- [[impl-trait|impl Trait]]
- [[option|Option]]
- [[result|Result<T, E>]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap]]

## Sources

- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
