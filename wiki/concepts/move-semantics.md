---
title: "Move Semantics"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, ownership]
source_count: 5
---

# Move Semantics

## Short Definition

Move heap-backed value ownershipini bir bindingdan boshqasiga o'tkazadi va oldingi bindingni invalid qiladi.

## Why It Matters

Move double free va use-after-free kabi memory safety bugsni compile time'da oldini oladi.

## Mental Model

```rust
let s1 = String::from("hello");
let s2 = s1;
```

Bu stack metadata'ni copy qiladi, heap data'ni emas. `s1` invalid bo'ladi, `s2` owner bo'ladi.

Struct update syntaxda ham move semantics amal qiladi: `..user1` orqali `String` field yangi instance'ga move bo'lishi mumkin.

String concatenationda `+` operatori left `String`ni move qiladi: `let s3 = s1 + &s2;`dan keyin `s1` invalid bo'ladi, `s2` valid qoladi.

Function call va return ham move nuqtalari:

```rust
fn greet(name: String) -> String {
    format!("Hello {}!!!", name)
}
```

Closure capture ham move nuqtasi bo'lishi mumkin:

```rust
let text = String::from("hello");
let consume = || println!("{}", text);
// `text` closure ichida by-value ishlatilsa, closure `FnOnce` bo'lishi mumkin
```

`move` keyword esa ownership transfer'ni explicit qiladi:

```rust
fn make_printer(text: String) -> impl Fn() {
    move || println!("{text}")
}
```

## Syntax and Examples

```rust
fn takes_ownership(some_string: String) {
    println!("{some_string}");
}

let s = String::from("hello");
takes_ownership(s);
// s is no longer valid here
```

## Common Mistakes

- Move bo'lgan value'ni qayta ishlatish.
- Move'ni shallow copy deb o'ylash; Rust oldingi bindingni invalid qilgani uchun bu Rust terminologyda move.
- Function call ownership transfer qilishini unutish.
- Struct update syntax fieldsni avtomatik clone qiladi deb o'ylash.
- `+` operatori stringlarni clone qiladi va oldingi left operand valid qoladi deb o'ylash.

## Related Concepts

- [[ownership]]
- [[copy-trait|Copy trait]]
- [[clone]]
- [[struct-update-syntax]]
- [[structs]]
- [[string-concatenation|string concatenation]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]

## Sources

- [[4-1-what-is-ownership]]
- [[5-1-defining-and-instantiating-structs]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
