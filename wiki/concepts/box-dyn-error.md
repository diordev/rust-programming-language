---
title: "Box<dyn Error>"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, traits]
source_count: 1
---

# Box<dyn Error>

## Short Definition

`Box<dyn Error>` beginner mental modelida "any kind of error" sifatida o'qilishi mumkin bo'lgan error return type.

## Why It Matters

`main` function `Result<(), Box<dyn Error>>` qaytarsa, `?` operator turli error typelarni early return qilishga imkon beradi. Bu Chapter 9.2da `main` ichida `File::open("hello.txt")?` ishlashi uchun ko'rsatiladi.

## Mental Model

`Box<dyn Error>` ortida [[traits|trait object]] bor; bu Chapter 18da chuqurlashadi. Hozircha uni "main turli errorlarni bitta return type orqali qaytara oladi" deb tushunish yetarli.

## Syntax and Examples

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

## Common Mistakes

- `Box<dyn Error>`ni har doim eng yaxshi app-level error design deb qabul qilish.
- `dyn Error` trait object ekanini keyingi trait object mavzusidan oldin chuqur tushunishga urinish.
- `Ok(())`ni unutish.

## Related Concepts

- [[main-function|main function]]
- [[question-mark-operator|question mark operator]]
- [[traits]]
- [[error-propagation|error propagation]]

## Sources

- [[9-2-recoverable-errors-with-result]]
