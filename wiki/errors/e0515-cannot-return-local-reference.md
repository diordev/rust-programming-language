---
title: "E0515 cannot return reference to local variable"
type: error
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, compiler-error, lifetimes, dangling-reference]
source_count: 1
---

# E0515 cannot return reference to local variable

## Symptom

```
error[E0515]: cannot return value referencing local variable `result`
```

Funksiya ichida yaratilgan (owned) valuega reference qaytarishga uringanda paydo bo'ladi. Funksiya tugaganda bu local value dropped bo'ladi — return qilingan reference dangling bo'lardi.

## Cause

Funksiya ichidagi local value funksiya tugaganda scope'dan chiqib drop bo'ladi. Unga reference qaytarish — dangling reference yaratish degani, Rust esa bunga hech qachon yo'l qo'ymaydi.

```rust
fn longest<'a>(x: &str, y: &str) -> &'a str {
    let result = String::from("really long string");
    result.as_str()  // E0515: result funksiya tugaganda dropped bo'ladi
}
```

Lifetime annotatsiya `'a` yozilsa ham bu muammoni hal qilmaydi — `result` parametrlar bilan hech qanday bog'liq emas.

## Fix Pattern

Owned value qaytarish kerak — shu funksiya chaqiruvchiga value'ning ownershippini o'tkazing:

```rust
fn create_string() -> String {
    String::from("really long string")
}
```

Chaqiruvchi kerak bo'lsa keyinchalik reference olishi mumkin:

```rust
fn main() {
    let s = create_string();
    let r = &s;       // bu yerda borrow qilish OK
    println!("{r}");
}
```

## Minimal Example

```rust
// Xato
fn make_ref<'a>() -> &'a str {
    let s = String::from("hello");
    s.as_str()          // E0515
}

// To'g'ri
fn make_owned() -> String {
    String::from("hello")
}
```

## Related Concepts

- [[lifetimes]]
- [[dangling-reference]]
- [[ownership]]
- [[e0597-does-not-live-long-enough]]
- [[e0106-missing-lifetime-specifier]]

## Sources

- [[10-3-validating-references-with-lifetimes-the-rust-programming-language]]
