---
title: "std::env::args"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cli, env, args, iterator]
source_count: 1
---

# std::env::args

## Short Definition

`std::env::args()` — Rust dasturiga berilgan command line argumentlarini qaytaruvchi standart kutubxona funksiyasi. Iterator qaytaradi; `collect()` bilan `Vec<String>`ga aylantiriladi.

## Why It Matters

CLI dasturlar doim tashqi argumentlarni qabul qilishi kerak. Rust'da bu juda oddiy tarzda amalga oshiriladi — `std::env` moduli orqali.

## Mental Model

`args()` — bu dasturga qo'yib yuborilgan "buyruqlar ro'yxati". Har bir argument alohida `String`:

```
$ minigrep hello world.txt
  args[0] = "target/debug/minigrep"  ← binary nomi (har doim birinchi)
  args[1] = "hello"                   ← birinchi argument
  args[2] = "world.txt"               ← ikkinchi argument
```

## Syntax and Examples

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    dbg!(&args);
}
```

Explicit `Vec<String>` type annotation **majburiy** — `collect()` qaysi turdagi collection kerakligini bila olmaydi.

### Import qilish usuli

```rust
use std::env;       // yaxshi: env::args() deyiladi
// use std::env::args; // yomonroq: args() deyiladi, lekin chalkash bo'lishi mumkin
```

Tavsiya: funksiya ikki yoki undan ko'p modul ichida bo'lsa, ota-modulni import qilish yaxshiroq (7-bob qoidasi).

### `args_os` — invalid Unicode uchun

```rust
use std::env;

fn main() {
    let args: Vec<std::ffi::OsString> = env::args_os().collect();
}
```

| | `args()` | `args_os()` |
|---|---|---|
| Qaytaradi | `Iterator<String>` | `Iterator<OsString>` |
| Unicode | Faqat valid UTF-8 | Har qanday platform bytes |
| Panic | Invalid Unicode bo'lsa | Yo'q |

## Common Mistakes

- `args[0]`ni argument sifatida ishlatish — bu binary nomi, argument emas
- `collect()` uchun type annotation unutish — compile error
- Argument soni tekshirmasdan `args[1]` ishlatish — index out of bounds panic

## Related Concepts

- [[iterators|iterators]]
- [[collect|collect()]]
- [[type-annotations|type annotations]]
- [[cli|CLI]]
- [[fs-read-to-string|fs::read_to-string]]

## Sources

- [[12-1-accepting-command-line-arguments-the-rust-programming-language]]
