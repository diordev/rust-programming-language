---
title: "Constructor Functions"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, associated-functions]
source_count: 2
---

# Constructor Functions

## Short Definition

Constructor-like associated functions yangi struct instance qaytaradigan helper functions.

## Why It Matters

Rustda `new` maxsus keyword yoki built-in constructor emas. Type authors `new`, `square`, yoki boshqa named associated functions yaratishi mumkin.

## Mental Model

Associated function instance talab qilmasa, type namespace orqali chaqiriladi va yangi value yaratishi mumkin.

## Syntax and Examples

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

let sq = Rectangle::square(3);
```

## `new` va `build` konventsiyasi

Rust'da muhim konventsiya: **`new` hech qachon muvaffaqiyatsiz bo'lmaydi** deb kutiladi. Agar constructor xato qaytarishi kerak bo'lsa, `new` emas, boshqa nom ishlatiladi — masalan `build`:

```rust
impl Config {
    // new — faqat muvaffaqiyatli holat
    fn new(query: String, file_path: String) -> Config {
        Config { query, file_path }
    }

    // build — muvaffaqiyatsiz bo'lishi mumkin, Result qaytaradi
    fn build(args: &[String]) -> Result<Config, &'static str> {
        if args.len() < 3 {
            return Err("not enough arguments");
        }
        Ok(Config {
            query: args[1].clone(),
            file_path: args[2].clone(),
        })
    }
}
```

`Config::build` — `minigrep` loyihasidagi real misol (12.3).

## Common Mistakes

- `new` nomi Rustda majburiy deb o'ylash.
- Constructor-like associated function method bo'lishi kerak deb o'ylash.
- Xato qaytarishi mumkin bo'lsa ham `new` ishlatish — `build` yoki boshqa nom tanlanishi kerak.

## Related Concepts

- [[associated-functions]]
- [[self-type|Self type]]
- [[struct-instances|struct instances]]
- [[result|Result]]
- [[unwrap-or-else|unwrap_or_else]]

## Sources

- [[5-3-methods-the-rust-programming-language]]
- [[12-3-refactoring-to-improve-modularity-and-error-handling-the-rust-programming-language]]
