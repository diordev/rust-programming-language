---
title: "Generic Type Parameters"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, generics]
source_count: 4
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

Backend beginner source muhim bir noaniqlikni yopadi: turli itemlardagi `T` bir xil harf bo'lsa ham, ular avtomatik bog'langan emas. `struct Holder<T>` ichidagi `T` bilan `fn make_holder<T>(v: T) -> Holder<T>` ichidagi `T` harf jihatdan bir xil, lekin scope bo'yicha alohida declaration. Istalsa `A`, `Element`, yoki boshqa nom bilan yozish mumkin.

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

Bir xil harfga bog'lanib qolmaslik:

```rust
struct Holder<Element> {
    v: Element,
}

fn make_holder<A>(v: A) -> Holder<A> {
    Holder { v }
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
- Bir itemdagi `T` boshqa itemdagi `T` bilan avtomatik bog'langan deb o'ylash.

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
- [[const-generics|const generics]]

## Sources

- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
- [[10-1-generic-data-types]]
- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/rust-for-backend-developers-generics]]
