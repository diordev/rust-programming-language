---
title: "ErrorKind"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, io]
source_count: 1
---

# ErrorKind

## Short Definition

`std::io::ErrorKind` I/O error sabablarini enum variants sifatida ifodalaydi, masalan `ErrorKind::NotFound`.

## Why It Matters

Hamma I/O errorlar bir xil recovery talab qilmaydi. File topilmasa create qilish mumkin, permission error bo'lsa boshqa action kerak bo'lishi mumkin.

## Mental Model

`io::Error` error detailini saqlaydi; `error.kind()` esa high-level category beradi. Shu category bo'yicha `match` qilib, har xil failure sabablariga har xil response beriladi.

## Syntax and Examples

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => File::create("hello.txt").unwrap(),
            _ => panic!("Problem opening the file: {error:?}"),
        },
    };
}
```

## Common Mistakes

- Barcha `Err` holatlarini bir xil deb ko'rish.
- `NotFound` recovery'sida `File::create` ham fail bo'lishi mumkinligini unutish.
- Error kind bilan exact error message'ni aralashtirish.

## Related Concepts

- [[io-error|io::Error]]
- [[match]]
- [[recoverable-errors|recoverable errors]]
- [[file-handle|file handle]]

## Sources

- [[9-2-recoverable-errors-with-result]]
