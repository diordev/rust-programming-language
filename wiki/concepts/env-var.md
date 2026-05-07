---
title: "env::var"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, environment-variables, std-env]
source_count: 1
---

# env::var

## Short Definition

`std::env::var(key)` — berilgan nom bo'yicha environment variableni o'qiydi va `Result<String, VarError>` qaytaradi.

## Why It Matters

CLI dasturlar konfiguratsiyasi uchun environment variable keng qo'llaniladi. Foydalanuvchi bir marta o'rnatadi, terminal sessiyasi davomida barcha chaqiruvlarda ishlaydi.

## Mental Model

```
O'rnatilgan: IGNORE_CASE=1  →  env::var("IGNORE_CASE")  →  Ok("1")
O'rnatilmagan             →  env::var("IGNORE_CASE")  →  Err(NotPresent)
```

Faqat o'rnatilganini bilish kerak bo'lsa `.is_ok()` yetarli — qiymatni `unwrap` qilish shart emas.

## Syntax and Examples

```rust
use std::env;

// Qiymatini olish
let val = env::var("MY_VAR").unwrap_or(String::from("default"));

// Faqat o'rnatilgan-o'rnatilmaganini tekshirish
let ignore_case = env::var("IGNORE_CASE").is_ok();

// Qiymat bilan ishlash
match env::var("PORT") {
    Ok(port) => println!("Port: {port}"),
    Err(_) => println!("PORT o'rnatilmagan"),
}
```

**Shell:**

```shell
# Linux/macOS — bir martalik
$ IGNORE_CASE=1 cargo run -- query file.txt

# Sessiya davomida
$ export IGNORE_CASE=1
$ cargo run -- query file.txt

# PowerShell
PS> $Env:IGNORE_CASE=1; cargo run -- query file.txt
PS> Remove-Item Env:IGNORE_CASE
```

## Common Mistakes

- `env::var` qiymatni `String` qilib qaytaradi. Unicode bo'lmagan OS nomlar uchun `env::var_os` + `OsString` kerak.
- `is_ok()` ishlatganda qiymat yo'qoladi — agar qiymat kerak bo'lsa `unwrap_or_else` yoki `match` ishlating.

## Related Concepts

- [[env-args|env::args]] — command line argumentlar
- [[result|Result]] — `Ok`/`Err` pattern
- [[standard-library|standard library]] — `std::env` moduli

## Sources

- [[12-5-working-with-environment-variables-the-rust-programming-language|12.5. Working with Environment Variables]]
