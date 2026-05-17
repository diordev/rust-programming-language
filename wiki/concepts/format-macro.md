---
title: "format! macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, strings, macros, formatting]
source_count: 2
---

# format! macro

## Short Definition

`format!` macro formatting syntax orqali `String` yaratadi; `println!`ga o'xshaydi, lekin outputni ekranga emas, return value sifatida beradi.

## Why It Matters

Bir nechta string yoki value'ni readable tarzda birlashtirishda `format!` `+` operatoridan qulayroq bo'lishi mumkin va argument ownershipini olmaydi.

## Mental Model

`format!` "formatlangan matnni `String` qilib qaytar" degani.

`println!` bilan syntax deyarli bir xil, lekin natija side effect emas, owned `String`.

## Syntax and Examples

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{s1}-{s2}-{s3}");
```

```rust
let s: String = format!("{} in the power of the 2 is {}", 3, 9);
```

## Common Mistakes

- `format!` stdoutga chiqaradi deb o'ylash; stdout uchun `println!`.
- Oddiy append yetarli bo'lgan joyda haddan tashqari formatting ishlatish.

## Related Concepts

- [[string-concatenation|string concatenation]]
- [[string-type|String]]
- [[println-macro|println! macro]]
- [[display-formatting|Display formatting]]
- [[macro]]

## Sources

- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[wiki/sources/rust-for-backend-developers-strings]]
