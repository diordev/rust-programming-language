---
title: "E0106 missing lifetime specifier"
type: error
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, compiler-error, lifetimes]
source_count: 3
---

# E0106 missing lifetime specifier

## Symptom

```
error[E0106]: missing lifetime specifier
```

Funksiya yoki struct reference qaytaradi yoki reference field saqlaydi, lekin compiler qaysi input bilan bog'liqligini aniqlay olmaydi.

## Cause

**Holat 1 — dangling return (Ch. 4.2):** `dangle` funksiyasi local `String`ga reference qaytarishga urinadi. Local value funksiya tugaganda dropped bo'ladi.

**Holat 2 — struct reference fields (Ch. 5.1):** Struct fieldlari `&str` bo'lsa, compiler bu references qancha yashashini struct type definitionidan bilishi kerak.

**Holat 3 — funksiya ikkita reference qabul qilib reference qaytaradi (Ch. 10.3):** Compiler qaysi parametrning lifetimesi qaytarilayotganini aniqlay olmaydi.

```rust
// Xato — Ch. 10.3
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}
// error[E0106]: help: consider introducing a named lifetime parameter
// fn longest<'a>(x: &'a str, y: &'a str) -> &'a str
```

## Fix Pattern

**Holat 1 — Owned value qaytaring:**

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}
```

**Holat 2 — Struct reference fields uchun:**

Beginner fix — owned field:

```rust
struct User {
    username: String,
}
```

Formal fix — lifetime annotation:

```rust
struct User<'a> {
    username: &'a str,
}
```

**Holat 3 — Funksiya ikkita reference parameter va return:**

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

## Minimal Example

```rust
// Failing — Ch 4.2
fn dangle() -> &String {
    let s = String::from("hello");
    &s
}

// Failing — Ch 10.3
fn longest(x: &str, y: &str) -> &str { x }

// Fixed
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str { x }
```

## Related Concepts

- [[lifetimes]]
- [[lifetime-elision]]
- [[dangling-reference|dangling reference]]
- [[reference]]
- [[ownership]]
- [[structs]]
- [[string-slice|String slice]]
- [[e0597-does-not-live-long-enough]]
- [[e0515-cannot-return-local-reference]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[10-3-validating-references-with-lifetimes-the-rust-programming-language]]
