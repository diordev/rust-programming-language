---
title: "BufReader"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, io, std, networking]
source_count: 1
---

# BufReader

## Short Definition

`std::io::BufReader<R>` - `Read` implement qiladigan oqim ustiga buffer qo'yib, undan samaraliroq va qulayroq o'qish imkonini beradigan wrapper type.

## Why It Matters

Chapter 21.1'da browser yuborgan HTTP request text satrlar ko'rinishida o'qiladi. `BufReader` bo'lmasa `TcpStream`dan raw bytes o'qib, newline'larni qo'lda ajratish kerak bo'lardi.

## Mental Model

`BufReader` oqimdan kichik-kichik o'qishlar o'rniga ichkarida bir buffer ushlab turadi. Dastur shu buffer ustida `lines()` kabi yuqori darajali metodlar bilan ishlaydi.

## Syntax and Examples

### `TcpStream` ustiga wrapper

```rust
use std::io::{BufRead, BufReader};

let buf_reader = BufReader::new(&stream);
let request_line = buf_reader.lines().next().unwrap().unwrap();
```

### Request'ni bo'sh satrgacha yig'ish

```rust
let http_request: Vec<_> = buf_reader
    .lines()
    .map(|result| result.unwrap())
    .take_while(|line| !line.is_empty())
    .collect();
```

## Common Mistakes

- **`lines()` raw newline'larni saqlaydi deb o'ylash**: u satrlarni ajratib beradi, separatorning o'zi natijaga kirmaydi.
- **`lines()` xato bermaydi deb o'ylash**: u `Result<String, io::Error>` iteratori.
- **Har oqim matnli bo'ladi deb qarash**: `lines()` text-based protocol uchun qulay, binary protocol uchun emas.

## Related Concepts

- [[tcp-stream]]
- [[http]]
- [[file-io|File I/O]]
- [[web-server]]

## Sources

- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
