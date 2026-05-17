---
title: "io::Error"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, error-handling, io]
source_count: 3
---

# io::Error

## Short Definition

`std::io::Error` I/O operation failure informationini saqlaydigan standard error type. `io::Result<T>` esa `Result<T, std::io::Error>` alias'i.

## Why It Matters

File, socket, stdin/stdout, directory scan kabi operationlar fail bo'lishi mumkin va shu failure modeli deyarli hammasida bir xil.

## Mental Model

`io::Error` "I/O operation nega fail bo'ldi?" degan ma'lumotni beradi. `kind()` orqali [[error-kind|ErrorKind]] olinadi. `Read`, `Write`, `File::open`, `read_dir` kabi API'lar shu error modeliga tayanadi.

## Syntax and Examples

```rust
use std::fs::File;
use std::io;

fn open_file() -> Result<File, io::Error> {
    File::open("hello.txt")
}
```

## Common Mistakes

- `io::Result`ni boshqa alohida error algebra deb o'ylash.
- `unwrap` bilan I/O failure'ni yashirish.
- Permission denied, not found, interrupted kabi farqli sabablarni bir xil deb ko'rish.

## Related Concepts

- [[result|Result<T, E>]]
- [[error-kind|ErrorKind]]
- [[error-propagation|error propagation]]
- [[read-trait|Read]]
- [[write-trait|Write]]
- [[file-io]]

## Sources

- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/rust-for-backend-developers-io]]
- [[wiki/sources/rust-for-backend-developers-file-system]]

