---
title: "Error Propagation"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, result]
source_count: 1
---

# Error Propagation

## Short Definition

Error propagation - function ichida errorni local handle qilmasdan, callerga `Err` sifatida qaytarish patterni.

## Why It Matters

Ba'zan function ichida to'g'ri recovery qarori uchun yetarli context bo'lmaydi. Caller esa userga message ko'rsatish, retry qilish, default value ishlatish, yoki programni to'xtatish qarorini yaxshiroq qila oladi.

## Mental Model

Propagation "men bu errorni ko'rdim, lekin uni hal qilish mening vazifam emas; caller hal qilsin" degani. Rustda bu odatda `Result<T, E>` return type va [[question-mark-operator|question mark operator]] bilan yoziladi.

## Syntax and Examples

Manual propagation:

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");

    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut username = String::new();

    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

With `?`:

```rust
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

## Common Mistakes

- Caller yaxshiroq qaror qila oladigan holatda function ichida `panic!` qilish.
- Function return type'ini `Result` qilmasdan `?` ishlatishga urinish.
- Propagation errorni "yo'q qildi" deb o'ylash; error callerga ko'chadi.

## Related Concepts

- [[result|Result<T, E>]]
- [[question-mark-operator|question mark operator]]
- [[recoverable-errors|recoverable errors]]
- [[io-error|io::Error]]
- [[from-trait|From trait]]

## Sources

- [[9-2-recoverable-errors-with-result]]
