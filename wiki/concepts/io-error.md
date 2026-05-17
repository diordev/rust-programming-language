---
title: "io::Error"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, io]
source_count: 1
---

# io::Error

## Short Definition

`std::io::Error` standard librarydagi I/O operation failure informationini saqlaydigan error type.

## Why It Matters

File ochish, file yaratish, yoki file o'qish kabi operations fail bo'lishi mumkin. `io::Error` failure sababini callerga `Result` ichida olib boradi.

## Mental Model

`io::Error` "I/O operation nega fail bo'ldi?" degan ma'lumotni beradi. `kind()` method orqali [[error-kind|ErrorKind]] olinadi.

## Syntax and Examples

```rust
use std::fs::File;
use std::io;

fn open_file() -> Result<File, io::Error> {
    File::open("hello.txt")
}
```

## Common Mistakes

- `io::Error` qaytishi mumkin bo'lgan operationlarni `unwrap` bilan yashirish.
- `io::Error`ni bitta sabab bilan cheklash; file missing, permission denied, interrupted read kabi turli sabablar bo'lishi mumkin.
- Errorni local handle qilish kerakmi yoki propagate qilish kerakmi, qaror bermaslik.

## Related Concepts

- [[result|Result<T, E>]]
- [[error-kind|ErrorKind]]
- [[error-propagation|error propagation]]
- [[file-handle|file handle]]

## Sources

- [[9-2-recoverable-errors-with-result]]
