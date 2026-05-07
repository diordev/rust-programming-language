---
title: "12.1. Accepting Command Line Arguments"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cli, env, args]
source_count: 1
---

# 12.1. Accepting Command Line Arguments

## Source

- Manba: The Rust Programming Language, 12.1
- URL: https://doc.rust-lang.org/stable/book/ch12-01-accepting-command-line-arguments.html

## Detailed Summary

`minigrep` loyihasi boshlangan. Dastlab, dastur command line argumentlarini qabul qilishi kerak. Ikkita argument: qidiriladigan matn (`query`) va fayl yo'li (`file_path`).

### `std::env::args` funksiyasi

Argumentlarni o'qish uchun `std::env` modulidan `args()` funksiyasi ishlatiladi:

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    dbg!(args);
}
```

**Muhim narsalar:**

1. `env::args()` — iterator qaytaradi
2. `.collect()` — iteratorni collectionga (bu holda `Vec<String>`) aylantiradi
3. `Vec<String>` uchun **explicit type annotation kerak** — Rust collection turini bila olmaydi

### `args[0]` — binary nomi

`args` vektorining birinchi elementi (`args[0]`) — dasturning o'zi nomi (binary path). Bu C tilidagi xulq-atvorga mos keladi. Bizning argumentlar `args[1]`dan boshlanadi:

```rust
let query = &args[1];
let file_path = &args[2];
```

### `args` vs `args_os`

| Funksiya | Qaytaradi | Ishlatilishi |
|---|---|---|
| `std::env::args()` | `Iterator<String>` | Oddiy Unicode argumentlar |
| `std::env::args_os()` | `Iterator<OsString>` | Invalid Unicode ham bo'lishi mumkin bo'lganda |

### Parent modulini import qilish

`std::env::args` ikki darajali modul ichida. Qoida: funksiya ota-moduliga ko'ra ikki yoki undan ko'p modul ichida bo'lsa, ota-modulni import qilish yaxshiroq:

```rust
use std::env;  // yaxshi — env::args() deyiladi
// use std::env::args; // yomonroq — args() deyiladi, lekin chalkash bo'lishi mumkin
```

## Key Concepts

- [[env-args|std::env::args]]
- [[iterators|iterators]]
- [[collect|collect()]]
- [[type-annotations|type annotations]]
- [[cli|command line arguments]]

## Code Examples

**Listing 12-1: argumentlarni vector sifatida yig'ish**

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    dbg!(args);
}
```

Ishga tushirish:

```shell
$ cargo run -- needle haystack
[src/main.rs:4:5] args = [
    "target/debug/minigrep",
    "needle",
    "haystack",
]
```

**Listing 12-2: argumentlarni o'zgaruvchilarga saqlash**

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();

    let query = &args[1];
    let file_path = &args[2];

    println!("Searching for {query}");
    println!("In file {file_path}");
}
```

## Exercises or Practice Ideas

- `args[0]`ni chiqarib, dastur nomiga qarab turli xulq-atvor ko'rsating.
- Kamida 2 ta argument berilmaganida xato chiqaruvchi qo'shimcha tekshirish yozing.
- `args_os()` bilan test qiling: argumentlarga invalid UTF-8 byte berish.

## Questions Raised

- `OsString` nima va `String`dan qanday farqi bor?
- Nima uchun `collect()` uchun explicit type annotation kerak?

## Links To Update

- [[env-args]]
- [[collect]]
- [[12-an-io-project]]
