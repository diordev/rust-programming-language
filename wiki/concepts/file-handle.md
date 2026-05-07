---
title: "File Handle"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, io, files]
source_count: 1
---

# File Handle

## Short Definition

File handle - opened file bilan keyingi read/write operations bajarish uchun ishlatiladigan value. Rustda `std::fs::File` file handle sifatida ishlatiladi.

## Why It Matters

`File::open` success bo'lsa `File` handle qaytaradi; failure bo'lsa [[io-error|io::Error]] qaytaradi. Shu sabab file access [[result|Result<T, E>]] orqali model qilinadi.

## Mental Model

File handle "file ochildi, endi undan foydalanish mumkin" degan resource value. U ownership va scope qoidalariga bo'ysunadi.

## Syntax and Examples

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
}
```

`greeting_file_result` success bo'lsa `Ok(File)` bo'ladi.

## Common Mistakes

- File path stringini file handle bilan aralashtirish.
- File open bo'lishi har doim success deb o'ylash.
- `File::create` ham fail bo'lishi mumkinligini unutish.

## Related Concepts

- [[io-error|io::Error]]
- [[result|Result<T, E>]]
- [[ownership]]
- [[error-handling]]

## Sources

- [[9-2-recoverable-errors-with-result-the-rust-programming-language]]
