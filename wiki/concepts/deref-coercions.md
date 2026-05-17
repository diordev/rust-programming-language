---
title: "Deref Coercions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, deref, strings, smart-pointers]
source_count: 3
---

# Deref Coercions

## Short Definition

Deref coercions Rust compiler reference typelarni function/method parameterlariga mosroq reference typega avtomatik coerce qiladigan feature.

## Why It Matters

Chapter 4.3da `fn first_word(s: &str) -> &str` functioniga `&String` ham berilishi mumkinligi shu feature bilan bog'liq. Chapter 15.2 bu mexanizmni [[deref-trait|Deref trait]] orqali to'liq ochadi.

## Mental Model

`String`dan whole string slice sifatida foydalanish mumkin:

```rust
let my_string = String::from("hello world");
let word = first_word(&my_string);
```

String concatenationda ham `&String` `&str`ga coerce bo'lishi mumkin: `s1 + &s2` expressionida right side `&s2[..]` kabi ishlatiladi.

Smart pointer misolida `&MyBox<String>` function call paytida `&String`, keyin `&str`ga coerce bo'lishi mumkin:

```rust
fn hello(name: &str) {
    println!("Hello, {name}!");
}

let m = MyBox::new(String::from("Rust"));
hello(&m);
```

## Syntax and Examples

```rust
fn first_word(s: &str) -> &str {
    &s[..]
}
```

## Common Mistakes

- Chapter 4 bosqichida deref coercionsni to'liq ownership/borrowing rule sifatida tushunishga urinish; bu keyinroq chuqurlashadi.
- `&String` va `&str` har doim aynan bir type deb o'ylash; coercion kerak joyda compiler yordam beradi.
- Deref coercion runtime conversion deb o'ylash; compiler kerakli `deref` callsni compile time'da joylaydi.
- `&T`dan `&mut U`ga coercion bo'ladi deb o'ylash. Rust bunday coercion qilmaydi.

## Related Concepts

- [[string-slice|String slice]]
- [[string-type|String]]
- [[slices]]
- [[string-concatenation|string concatenation]]
- [[deref-trait|Deref trait]]
- [[deref-mut-trait|DerefMut trait]]
- [[smart-pointers]]
- [[box-t|Box<T>]]

## Sources

- [[4-3-the-slice-type]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[15-2-treating-smart-pointers-like-regular-references]]
