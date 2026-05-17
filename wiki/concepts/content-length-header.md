---
title: "Content-Length Header"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, http]
source_count: 1
---

# Content-Length Header

## Short Definition

HTTP response body uzunligini **byte**larda ko'rsatadigan header: `Content-Length: N`.

## Why It Matters

Browser response body qayerda tugashini bilishi kerak. Server HTML yoki boshqa body yuborayotgan bo'lsa, `Content-Length` client'ga qancha byte o'qish kerakligini aniq bildiradi.

## Mental Model

Status line "javobning turi"ni aytsa, `Content-Length` "javobning hajmi"ni aytadi. Bu protocol contract'ining bir qismi.

## Syntax and Examples

### Rust'da header yasash

```rust
let contents = std::fs::read_to_string("hello.html").unwrap();
let length = contents.len();
let response =
    format!("HTTP/1.1 200 OK\r\nContent-Length: {length}\r\n\r\n{contents}");
```

### Eslatma

`String::len()` Rust'da byte sonini beradi. HTTP uchun aynan byte length kerak.

## Common Mistakes

- **Character count bilan byte count'ni aralashtirish**: HTTP byte uzunlikni kutadi.
- **Header va body orasidagi bo'sh qatorni unutish**: `\r\n\r\n` bo'lmasa body noto'g'ri parse qilinadi.
- **Body bor response'ga header qo'ymaslik**: browser xulqini noaniq qiladi.

## Related Concepts

- [[http]]
- [[web-server]]
- [[format-macro|format! macro]]
- [[fs-read-to-string|fs::read_to_string]]

## Sources

- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
