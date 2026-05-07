---
title: "Deref Coercions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, deref, strings]
source_count: 2
---

# Deref Coercions

## Short Definition

Deref coercions Rust compiler reference typelarni function/method parameterlariga mosroq reference typega avtomatik coerce qiladigan feature.

## Why It Matters

Chapter 4.3da `fn first_word(s: &str) -> &str` functioniga `&String` ham berilishi mumkinligi shu feature bilan bog'liq. Batafsil tushuntirish Chapter 15da keladi.

## Mental Model

`String`dan whole string slice sifatida foydalanish mumkin:

```rust
let my_string = String::from("hello world");
let word = first_word(&my_string);
```

String concatenationda ham `&String` `&str`ga coerce bo'lishi mumkin: `s1 + &s2` expressionida right side `&s2[..]` kabi ishlatiladi.

## Syntax and Examples

```rust
fn first_word(s: &str) -> &str {
    &s[..]
}
```

## Common Mistakes

- Chapter 4 bosqichida deref coercionsni to'liq ownership/borrowing rule sifatida tushunishga urinish; bu keyinroq chuqurlashadi.
- `&String` va `&str` har doim aynan bir type deb o'ylash; coercion kerak joyda compiler yordam beradi.

## Related Concepts

- [[string-slice|String slice]]
- [[string-type|String]]
- [[slices]]
- [[string-concatenation|string concatenation]]

## Sources

- [[4-3-the-slice-type-the-rust-programming-language]]
- [[8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language]]
