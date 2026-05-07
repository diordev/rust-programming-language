---
title: "separation of concerns"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, design, refactoring, architecture]
source_count: 2
---

# separation of concerns

## Short Definition

Har bir funksiya yoki modul faqat bitta aniq vazifani bajarishi kerak. Bir funksiya bir vaqtda ko'p ish qilsa, kod o'sishi bilan uni tuzatish va test qilish qiyinlashadi.

## Why It Matters

`minigrep` misolida `main` funksiyasi boshlanganda bir vaqtda:
- Argumentlarni tahlil qiladi
- Faylni o'qiydi
- Qidiradi
- Natijani chiqaradi

Bu qisqa dasturlarda ishlaydi, lekin kod o'sishi bilan:
- Funksiyani tushunish qiyinlashadi
- Testlar yozish qiyinlashadi
- Xatolarni topish qiyinlashadi

## Mental Model

"Single responsibility principle" bilan bog'liq. Har bir funksiyaningki bitta sababi bo'lishi kerak.

`main` funksiyasining ideal roli: boshqa funksiyalarni chaqirib, ularni birlashtirish. Haqiqiy logika alohida funksiya/modullarda.

## Syntax and Examples

**Yomon (hamma narsa `main` da):**

```rust
fn main() {
    let args: Vec<String> = env::args().collect();
    let query = &args[1];
    let file_path = &args[2];
    let contents = fs::read_to_string(file_path).expect("...");
    // qidirish kodi...
    // chiqarish kodi...
}
```

**Yaxshi (12.3 dan keyin to'liq refactored holat):**

```rust
// src/main.rs — faqat entry point
use std::process;
use minigrep::run;

fn main() {
    let args: Vec<String> = env::args().collect();

    let config = Config::build(&args).unwrap_or_else(|err| {
        println!("Problem parsing arguments: {err}");
        process::exit(1);
    });

    if let Err(e) = run(config) {
        println!("Application error: {e}");
        process::exit(1);
    }
}

// src/lib.rs — barcha logika
pub struct Config { pub query: String, pub file_path: String }

impl Config {
    pub fn build(args: &[String]) -> Result<Config, &'static str> {
        if args.len() < 3 { return Err("not enough arguments"); }
        Ok(Config { query: args[1].clone(), file_path: args[2].clone() })
    }
}

pub fn run(config: Config) -> Result<(), Box<dyn std::error::Error>> {
    let contents = std::fs::read_to_string(config.file_path)?;
    println!("{contents}");
    Ok(())
}
```

### Binary project uchun qoida:

`main` funksiyasida faqat:
1. Command line parsing logikasini chaqirish
2. Boshqa konfiguratsiyani sozlash
3. `lib.rs` dagi `run` funksiyasini chaqirish
4. `run` xatosini ishlash

Qolgan hamma narsa `lib.rs` da — u yerda test yozish mumkin.

## Common Mistakes

- Kichik dasturda hamma narsani `main`ga yozish — keyinchalik refactor qilish qiyin
- Funksiyalar juda ko'p vazifa bajarsa, ularni test qilib bo'lmaydi

## Related Concepts

- [[refactoring|refactoring]]
- [[result|Result]]
- [[error-handling|error handling]]
- [[testing|testing]]
- [[functions|functions]]

## Sources

- [[12-2-reading-a-file-the-rust-programming-language]]
- [[12-an-io-project-the-rust-programming-language]]
- [[12-3-refactoring-to-improve-modularity-and-error-handling-the-rust-programming-language]]
