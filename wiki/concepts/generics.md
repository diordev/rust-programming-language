---
title: "Generics"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, generics]
source_count: 7
---

# Generics

## Short Definition

`generics` bir xil logicni turli typelar bilan ishlatish uchun type parameterlardan foydalanish usuli. `<T>` syntax — eng keng uchraydigan generic parameter belgisi.

## Why It Matters

Generics duplicationni kamaytiradi va Rustga abstractionni compile time'da tekshirish imkonini beradi. Standard library generics'siz mavjud bo'la olmaydi: [[option|Option<T>]], `Result<T, E>`, `Vec<T>`, `HashMap<K, V>` — barchasi generic typelar.

## Mental Model

Generic code "hozircha type nomi aniq emas, lekin qoidalari aniq" degan shaklda yoziladi. Kerakli behavior odatda [[traits]] orqali cheklanadi.

Har bir concrete `T` generic typeni *boshqa type* qiladi: `Option<i32>` va `Option<String>` bir-biriga implicit cast bo'lmaydi. Compiler har birini alohida type sifatida tekshiradi.

`Result<T, E>` ikkita generic parameter ishlatadi: `T` success value type'i, `E` error value type'i. Masalan `Result<String, io::Error>` success bo'lsa `String`, failure bo'lsa `io::Error` qaytaradi.

Chapter 10 genericsni duplicationni kamaytirish vositasi sifatida rasmiy boshlaydi. Function extraction value-level abstraction bo'lsa, generics type-level abstraction beradi.

Chapter 10.1 genericsni definitions ichida ko'rsatadi: `fn largest<T>(list: &[T]) -> &T`, `struct Point<T>`, `enum Option<T>`, `enum Result<T, E>`, va `impl<T> Point<T>`. Generic parameterlar ishlatilishidan oldin `<>` ichida e'lon qilinadi.

Generic code body ishlatadigan behavior signature'da ko'rinishi kerak. Masalan `largest<T>` ichida `>` ishlatilsa, `T` [[partial-ord|PartialOrd]] implement qilishi kerak. Aks holda compiler [[e0369-binary-operation-cannot-be-applied|E0369]] beradi.

Rust generics runtime'da sekinlashtirmaydi: compiler [[monomorphization]] orqali generic code'ni ishlatilgan concrete typelar bo'yicha specialized codega aylantiradi.

Chapter 10.2 generic code'ni [[trait-bounds|trait bounds]] bilan bog'laydi. `T: Summary`, `T: Summary + Display`, va `where` clauses generic type qaysi behaviorni support qilishi kerakligini compile time'da bildiradi.

## Syntax and Examples

Generic function:

```rust
fn identity<T>(value: T) -> T {
    value
}
```

Generic enum (standard library `Option`):

```rust
enum Option<T> {
    None,
    Some(T),
}

let some_number: Option<i32> = Some(5);
let some_char: Option<char> = Some('e');
let absent_number: Option<i32> = None; // annotation kerak
```

`absent_number`'ga annotation kerak — `None`'dan compiler `T`'ni infer qila olmaydi.

Standard library vector:

```rust
let v: Vec<i32> = Vec::new();
```

`Vec<T>`dagi `T` element type'ini bildiradi. `Vec<i32>` va `Vec<String>` boshqa-boshqa concrete typelar.

Generic `Result`:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Generic type parameters examples:

```rust
Option<T>
Vec<T>
HashMap<K, V>
Result<T, E>
```

Generic function with a trait bound:

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

Generic struct:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}
```

Generic method:

```rust
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
```

Generic function constrained by a trait:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Multiple bounds:

```rust
pub fn notify<T: Summary + Display>(item: &T) {
    println!("Breaking news! {}", item.summarize());
    println!("{}", item);
}
```

## Common Mistakes

- Generic type parameter hamma operationlarni avtomatik qo'llaydi deb o'ylash.
- Trait bound kerak bo'lgan joyda uni yozmaslik.
- `T`ni ishlatib, lekin `fn name<T>` yoki `impl<T>`da e'lon qilmaslik.
- Bitta `T` ishlatilgan fields bitta instance ichida bir xil concrete type bo'lishini unutish.
- Bo'sh `None` (yoki shunga o'xshash) holatda type annotation talabini hisobga olmaslik.
- Empty `Vec::new()` uchun compiler har doim `T`ni topadi deb o'ylash.
- Function extraction va generic abstraction orasidagi farqni ko'rmaslik.
- Generics runtime dispatch bo'ladi deb o'ylash; Rust ko'p generic code'ni monomorphization qiladi.
- Trait bound generic type relationshipni ifodalashi mumkinligini unutish, masalan ikki parameter bir xil `T` bo'lishi.
- Return-position `impl Trait` generic abstraction bo'lsa ham bitta concrete return type talab qilishini unutish.

## Related Concepts

- [[traits]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-enums|generic enums]]
- [[generic-methods|generic methods]]
- [[partial-ord|PartialOrd]]
- [[trait-bounds|trait bounds]]
- [[impl-trait|impl Trait]]
- [[where-clauses|where clauses]]
- [[conditional-method-implementations|conditional method implementations]]
- [[monomorphization]]
- [[abstraction]]
- [[code-duplication|code duplication]]
- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[option|Option]]
- [[result|Result<T, E>]]
- [[enums]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]

## Sources

- [[0-2-introduction-the-rust-programming-language]]
- [[6-1-defining-an-enum-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[9-2-recoverable-errors-with-result-the-rust-programming-language]]
- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
