---
title: "12.2. Reading a File"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, fs, file-io, cli]
source_count: 1
---

# 12.2. Reading a File

## Source

- Manba: The Rust Programming Language, 12.2
- URL: https://doc.rust-lang.org/stable/book/ch12-02-reading-a-file.html

## Detailed Summary

`minigrep`ning keyingi qadami — `file_path` argumentida ko'rsatilgan faylni o'qish. Buning uchun `std::fs` moduli ishlatiladi.

### `fs::read_to_string`

```rust
use std::env;
use std::fs;

fn main() {
    let args: Vec<String> = env::args().collect();

    let query = &args[1];
    let file_path = &args[2];

    println!("Searching for {query}");
    println!("In file {file_path}");

    let contents = fs::read_to_string(file_path)
        .expect("Should have been able to read the file");

    println!("With text:\n{contents}");
}
```

`fs::read_to_string(file_path)`:
- Faylni ochadi
- Butun matnini `String`ga o'qiydi
- `std::io::Result<String>` qaytaradi
- `.expect()` bilan xato bo'lganda panic

### Test fayli: `poem.txt`

Test uchun Emily Dickinson she'ri ishlatiladi:

```
I'm nobody! Who are you?
Are you nobody, too?
Then there's a pair of us - don't tell!
They'd banish us, you know.

How dreary to be somebody!
How public, like a frog
To tell your name the livelong day
To an admiring bog!
```

### Muammo: `main` da haddan ko'p responsibility

Hozir `main` funksiyasi bir vaqtda:
1. Argumentlarni tahlil qiladi
2. Faylni o'qiydi
3. (Keyinchalik) qidiradi
4. (Keyinchalik) natijani chiqaradi

Bu **separation of concerns** prinsipini buzadi. Kichik dasturlarda muammo bo'lmasa ham, dastur o'sishi bilan tuzatish qiyinlashadi. Keyingi bo'limlarda refactoring qilinadi.

### Xato ishlash ham zaif

`.expect()` ishlatilgan — bu ishlaydi, lekin xato xabarlari etarlicha aniq emas. Masalan, fayl topilmasa:

```
thread 'main' panicked at 'Should have been able to read the file: ...'
```

Keyinchalik bu yanada yaxshilanadi.

## Key Concepts

- [[fs-read-to-string|fs::read_to_string]]
- [[result|Result<String>]]
- [[expect|expect()]]
- [[separation-of-concerns|separation of concerns]]
- [[file-io|File I/O]]
- [[separation-of-concerns|single responsibility principle]]

## Code Examples

**Listing 12-4: faylni o'qish**

```rust
use std::env;
use std::fs;

fn main() {
    let args: Vec<String> = env::args().collect();

    let query = &args[1];
    let file_path = &args[2];

    println!("Searching for {query}");
    println!("In file {file_path}");

    let contents = fs::read_to_string(file_path)
        .expect("Should have been able to read the file");

    println!("With text:\n{contents}");
}
```

Ishga tushirish:

```shell
$ cargo run -- the poem.txt
Searching for the
In file poem.txt
With text:
I'm nobody! Who are you?
...
```

## Exercises or Practice Ideas

- Fayl mavjud bo'lmasa xatoni `expect` o'rniga `match` bilan ushlab ko'ring.
- Katta fayl bilan `read_to_string` ishlashini test qiling.
- `main`ni avvaldan ikki funksiyaga ajrating: biri argumentlarni tahlil qilsin, biri faylni o'qisin.

## Questions Raised

- `BufReader` va `read_to_string` qanday farq qiladi? (katta fayllar uchun qaysi samarali?)
- Keyingi bo'limlarda `main` qanday refactor qilinadi?

## Links To Update

- [[fs-read-to-string]]
- [[separation-of-concerns]]
- [[12-an-io-project]]
