---
title: "String"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, string, ownership]
source_count: 5
---

# String

## Short Definition

`String` Rust standard librarydagi growable, UTF-8 encoded text type. U heap allocation ishlatadi.

## Why It Matters

`String` ownershipni tushuntirish uchun asosiy example: heap data borligi sababli assignmentda move bo'ladi, automatic deep copy qilinmaydi.

## Mental Model

`String` stackda pointer, length, capacity metadata saqlaydi; text bytes heapda turadi. `String::from("hello")` heap allocation qiladi.

Struct fieldsda `String` ishlatish instance data'ga ownerlik qilishini bildiradi. Bu `&str` reference fieldlardan soddaroq, chunki reference fields uchun [[lifetimes]] kerak.

Chapter 8 `String`ni common [[collections|collection]]lardan biri sifatida qayta ochadi: u growable UTF-8 text collection. `String` `Vec<u8>` ustidagi wrapper bo'lib, valid [[utf-8|UTF-8]] guarantees, restrictions, va text-specific methods beradi.

Rustda "string" deganda `String` yoki [[string-slice|`&str`]] nazarda tutilishi mumkin. `String` owned/mutable/growable; `&str` borrowed view.

Backend beginner source `String`ning amaliy constructor setini juda aniq beradi: `String::from("text")`, `String::new()`, va `"text".to_string()`. Shuningdek `as_str()` borrowed view'ga qaytish uchun natural bridge ekanini ko'rsatadi.

## Syntax and Examples

```rust
let mut s = String::from("hello");
s.push_str(", world!");
println!("{s}");
```

Creation:

```rust
let s = String::new();
let s = "initial contents".to_string();
let s = String::from("initial contents");
```

Borrowed view:

```rust
let owned = String::from("text");
let borrowed: &str = owned.as_str();
```

Append:

```rust
let mut s = String::from("lo");
s.push('l');
```

Concatenation:

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2;
```

Move:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

Clone:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```

## Common Mistakes

- `String`ni string literal bilan adashtirish.
- Assignmentdan keyin oldingi binding valid qoladi deb o'ylash.
- `clone()` performance costini ko'rmaslik.
- Struct field uchun `&str` ishlatish lifetime complexity olib kelishini hisobga olmaslik.
- `String` collection emas deb o'ylash; u growable text data structure.
- `.len()` human character count qaytaradi deb o'ylash; u bytes count.
- `String`ni integer index bilan ishlatish mumkin deb kutish.

## Related Concepts

- [[string-literal-vs-string]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[clone]]
- [[drop]]
- [[structs]]
- [[lifetimes]]
- [[collections]]
- [[standard-library|standard library]]
- [[utf-8|UTF-8]]
- [[string-slice|String slice]]
- [[string-concatenation|string concatenation]]
- [[string-indexing|string indexing]]
- [[format-macro|format! macro]]

## Sources

- [[4-1-what-is-ownership]]
- [[5-1-defining-and-instantiating-structs]]
- [[wiki/sources/8-common-collections]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[wiki/sources/rust-for-backend-developers-strings]]
