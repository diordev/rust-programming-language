---
title: "fs::read_to_string"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, fs, file-io, result]
source_count: 1
---

# fs::read_to_string

## Short Definition

`std::fs::read_to_string(path)` — berilgan fayl yo'li bo'yicha faylni ochib, uning butun mazmunini `String`ga o'qib `std::io::Result<String>` qaytaradi.

## Why It Matters

Fayl o'qish CLI dasturlarda juda keng tarqalgan talab. `read_to_string` — bu eng sodda va to'g'ridan-to'g'ri usul: bir chaqiruv bilan faylni butun o'qib olish.

## Mental Model

Fayl disk ustida joylashgan. `fs::read_to_string`:
1. Faylni ochadi
2. Butun matnini xotiraga o'qiydi (`String`ga)
3. Fayl yopiladi
4. `Result<String>` qaytaradi — muvaffaqiyat yoki xato

## Syntax and Examples

```rust
use std::fs;

let contents = fs::read_to_string("poem.txt")
    .expect("Should have been able to read the file");
```

Yoki `Result`ni to'liq ishlash:

```rust
use std::fs;
use std::io;

fn read_poem() -> Result<String, io::Error> {
    fs::read_to_string("poem.txt")
}
```

`?` operatori bilan:

```rust
use std::fs;
use std::io;

fn read_poem() -> Result<String, io::Error> {
    let contents = fs::read_to_string("poem.txt")?;
    Ok(contents)
}
```

## Common Mistakes

- Katta fayllarni `read_to_string` bilan o'qish — bu butun faylni xotiraga yuklaydi. Katta fayllar uchun `BufReader` yaxshiroq.
- Fayl yo'li noto'g'ri bo'lganda `expect` yaxshi xato xabari bermaydi — maxsus `match` yaxshiroq.

## Related Concepts

- [[result|Result]]
- [[expect|expect()]]
- [[question-mark-operator|? operatori]]
- [[io-error|io::Error]]
- [[separation-of-concerns|separation of concerns]]
- [[env-args|std::env::args]]

## Sources

- [[12-2-reading-a-file]]
