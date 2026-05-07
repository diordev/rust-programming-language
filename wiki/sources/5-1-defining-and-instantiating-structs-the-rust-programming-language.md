---
title: "5.1. Defining and Instantiating Structs - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, structs]
source_count: 1
source_path: "raw/books/the_rust_programming_language/5.1. Defining and Instantiating Structs - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch05-01-defining-structs.html"
---

# 5.1. Defining and Instantiating Structs - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/5.1. Defining and Instantiating Structs - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch05-01-defining-structs.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[structs]]ni define qilish, instance yaratish, fieldlarga kirish, va related shorthand syntaxlarni tushuntiradi.

Structs [[tuple]]ga o'xshaydi: ikkalasi ham related valuesni group qiladi va field types turlicha bo'lishi mumkin. Farq shundaki, struct har bir data piece uchun field name beradi. Bu instance yaratish va field accessni orderga emas, nomga bog'laydi. Shu sababli structs tuplega qaraganda flexible va readableroq.

Struct definition `struct` keyword, type name, va curly braces ichidagi [[struct-fields|fields]]dan iborat. `User` exampleida fields:

- `active: bool`
- `username: String`
- `email: String`
- `sign_in_count: u64`

Struct instance type template'ni concrete values bilan to'ldiradi. Instance yaratishda field order definitiondagi order bilan bir xil bo'lishi shart emas, chunki fields nomlanadi. Field value olish uchun dot notation ishlatiladi: `user1.email`.

Mutation instance darajasida bo'ladi. Agar `let mut user1 = User { ... }` bo'lsa, `user1.email = ...` mumkin. Lekin Rust faqat ayrim fieldlarni mutable deb belgilashga ruxsat bermaydi; butun instance mutable yoki immutable bo'ladi.

Function body oxirida struct literal expression yozilsa, function struct instance qaytarishi mumkin. `build_user(email, username) -> User` exampleida `active` va `sign_in_count` fixed default-like values oladi, `email` va `username` esa parametersdan keladi.

[[field-init-shorthand]] repetitionni kamaytiradi. Agar field name va local variable/parameter name bir xil bo'lsa, `email: email` o'rniga `email` yoziladi.

[[struct-update-syntax]] yangi instance yaratishda boshqa instance fieldsidan foydalanadi. `User { email: String::from("another@example.com"), ..user1 }` explicit berilmagan fieldsni `user1`dan oladi. `..user1` oxirida kelishi kerak. Bu syntax assignmentga o'xshab `=` ishlatadi va ownership semanticsga bo'ysunadi: `String` field move bo'lsa, eski instance qisman invalid bo'lishi mumkin. Exampleda `username: String` `user1`dan `user2`ga move bo'lgani uchun `user1` butunicha ishlatilmaydi; lekin `user1.email` hali ishlatilishi mumkin, chunki u move qilinmagan. `active: bool` va `sign_in_count: u64` [[copy-trait|Copy trait]] types bo'lgani uchun copy qilinadi.

[[tuple-structs]] tuplega o'xshash, lekin struct name orqali distinct type yaratadi. `struct Color(i32, i32, i32);` va `struct Point(i32, i32, i32);` bir xil field typesga ega bo'lsa ham, Rust uchun boshqa-boshqa types. Tuple struct instances destructure qilinishi va index orqali access qilinishi mumkin, lekin destructuringda type name ham yoziladi: `let Point(x, y, z) = origin;`.

[[unit-like-structs]] fieldlarga ega emas va `()` unit typega o'xshaydi. `struct AlwaysEqual;` datasiz type yaratadi. Bunday type keyinchalik data saqlamasdan [[traits]] implement qilish kerak bo'lganda foydali bo'lishi mumkin.

Ownership of struct data section muhim: `User` fields uchun `String` ishlatilishi deliberate choice, chunki har bir `User` instance o'z data'siga own qilishi va data instance valid bo'lgancha valid qolishi kerak. Struct ichida `&str` references saqlash mumkin, lekin buning uchun [[lifetimes]] kerak. Lifetime annotationsiz `username: &str` va `email: &str` fields compilerda [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]] beradi. Hozirgi safe beginner pattern: struct data uchun owned `String` ishlatish; references-in-structs keyin Chapter 10da formal lifetime syntax bilan o'rganiladi.

## Key Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[struct-update-syntax]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[lifetimes]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]

## Code Examples

Struct definition:

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Struct instance:

```rust
let user1 = User {
    active: true,
    username: String::from("someusername123"),
    email: String::from("someone@example.com"),
    sign_in_count: 1,
};
```

Mutable field update through mutable instance:

```rust
let mut user1 = User {
    active: true,
    username: String::from("someusername123"),
    email: String::from("someone@example.com"),
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");
```

Field init shorthand:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

Struct update syntax:

```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1
};
```

Tuple structs:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

Unit-like struct:

```rust
struct AlwaysEqual;

let subject = AlwaysEqual;
```

Reference fields without lifetimes fail:

```rust
struct User {
    active: bool,
    username: &str,
    email: &str,
    sign_in_count: u64,
}
```

## Exercises or Practice Ideas

- `User` structini yozib, field orderni almashtirib instance yaratish.
- `let mut user1` bilan `email` fieldini update qilish; `mut`ni olib tashlab compiler errorni ko'rish.
- `build_user`ni avval full `email: email`, keyin field init shorthand bilan yozish.
- Struct update syntax ishlatib `String` field move bo'lgandan keyin `user1`ni ishlatishga urinib compiler errorni kuzatish.
- `Color` va `Point` tuple structs yaratib, function parameter type farqini sinab ko'rish.
- `User` structida `&str` fields yozib [[e0106-missing-lifetime-specifier|E0106]]ni ko'rish, keyin `String`ga qaytarib tuzatish.

## Questions Raised

- Struct update syntax qaysi fieldsni move qiladi va qaysilarini copy qiladi?
- Struct ichida owned `String` ishlatish bilan borrowed `&str` ishlatish API va lifetime complexityni qanday o'zgartiradi?
- Tuple struct qachon named fieldsli structdan yaxshiroq?
- Unit-like structning practical trait implementation use case'lari qanday?

## Links To Update

- [[5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]]
- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[struct-update-syntax]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[lifetimes]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]
