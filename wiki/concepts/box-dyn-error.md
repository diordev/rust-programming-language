---
title: "Box<dyn Error>"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-18
tags: [rust, error-handling, traits]
source_count: 2
---

# Box<dyn Error>

## Short Definition

`Box<dyn Error>` beginner mental modelida "any kind of error" sifatida o'qilishi mumkin bo'lgan error return type.

## Why It Matters

`main` function `Result<(), Box<dyn Error>>` qaytarsa, `?` operator turli error typelarni early return qilishga imkon beradi. Bu Chapter 9.2da `main` ichida `File::open("hello.txt")?` ishlashi uchun ko'rsatiladi.

## Mental Model

`Box<dyn Error>` ortida [[traits|trait object]] bor; bu Chapter 18da chuqurlashadi. Hozircha uni "turli concrete error'larni bitta erased boundary orqali qaytarish" deb tushunish yetarli.

Backend source bu type'ning app-level foydasini aniq ko'rsatadi: har error turi bo'yicha maxsus recovery yo'q bo'lsa, typed wrapping'ni davom ettirish o'rniga log + generic response yetarli bo'lishi mumkin.

## Syntax and Examples

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

Downcast bilan selected concrete type'ni ajratish ham mumkin:

```rust
if let Some(err_a) = e.downcast_ref::<ErrA>() {
    println!("Handle ErrA separately: {err_a}");
}
```

## Common Mistakes

- `Box<dyn Error>`ni har doim eng yaxshi app-level error design deb qabul qilish.
- `dyn Error` trait object ekanini keyingi trait object mavzusidan oldin chuqur tushunishga urinish.
- `Ok(())`ni unutish.
- Erased error qaytarib, keyin ko'p joyda manual downcast qilish.

## Related Concepts

- [[main-function|main function]]
- [[question-mark-operator|question mark operator]]
- [[traits]]
- [[error-propagation|error propagation]]
- [[error-downcasting]]
- [[wiki/crates/anyhow]]

## Sources

- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/rust-for-backend-developers-error-handling]]
