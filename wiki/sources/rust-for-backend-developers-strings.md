---
title: "Строки - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, strings]
source_count: 1
---

# Строки - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/16. Строки.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/strings.html

## Detailed Summary

Bu source Rustdagi string modelini `&str` va `String` chegarasi orqali tushuntiradi. Asosiy message oddiy: ikkalasi ham UTF-8 text bilan ishlaydi, lekin ownership modelida keskin farq bor.

`&str` borrowed string slice. String literal double quotes ichida yozilganda type odatda `&str` bo'ladi. Source buni "pointer + length bo'lgan UTF-8 slice" sifatida tushuntiradi. Muhim detail: `&str` xotirani boshqarmaydi; u static data, heapdagi `String` bufferi, yoki boshqa joydagi textga qarashi mumkin.

`String` esa buffer egasi. Source uni `Vec<u8>` ustidagi wrapper deb beradi: stackda metadata, heapda UTF-8 bytes. Shundan kelib chiqib `String` always owned heap buffer bilan bog'lanadi va `&str` slice'ini o'zidan bera oladi.

Creation uchun uchta yo'l ko'rsatiladi: `String::from(&str)`, `String::new()`, va `to_string()`. Source `String::from`da literal yoki boshqa `&str`dan heapga copy bo'lishini, `String::new()` esa bo'sh owned buffer yaratishini ko'rsatadi. `push(char)` va `push_str(&str)` bilan append qilish ham ko'rsatiladi.

`a_string.as_str()` orqali owned `String`dan borrowed `&str` olish practical bridge sifatida beriladi. Oxirida `format!` macro `println!` bilan bir xil formatting syntax ishlatib, console'ga chiqarmasdan `String` qaytarishi ko'rsatiladi.

Source o'zi ham ogohlantiradi: ownership va structs boblaridan oldin string material qisman og'ir ko'rinishi mumkin. Bu to'g'ri; string chapter aslida ownership, slices, UTF-8, va collections kesishgan nuqta.

## Key Concepts

- [[string-type|String]]
- [[string-slice|String slice]]
- [[string-literal]]
- [[utf-8|UTF-8]]
- [[format-macro|format! macro]]
- [[slices]]
- [[vector|Vec<T>]]
- [[ownership]]

## Code Examples

```rust
let s: &str = "some text";
let owned = String::from(s);
let borrowed_again: &str = owned.as_str();
```

```rust
let mut buf = String::new();
buf.push('H');
buf.push_str("ello");
```

```rust
let s: String = "text".to_string();
let formatted = format!("{} in the power of 2 is {}", 3, 9);
```

## Exercises or Practice Ideas

- Bir xil text uchun `&str`, `String::from`, va `.to_string()` variantlarini yonma-yon yozing.
- `String::new()` + `push`/`push_str` bilan console input buffer mental modelini tushuntiring.
- `format!` qachon `push_str`dan qulayroq ekanini misol bilan ko'rsating.

## Questions Raised

- `&str` parameter qachon `String` parameterdan tozaroq API beradi?
- `String::from` va `.to_string()` orasidagi farq syntaxdami yoki semanticsdami?
- `String`ning `Vec<u8>` ustidagi wrapper ekanini qaysi nuqtagacha o'quvchiga ochish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-strings]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[format-macro|format! macro]]
