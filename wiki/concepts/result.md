---
title: "Result"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, error-handling]
source_count: 6
---

# Result

## Short Definition

`Result<T, E>` recoverable success/failureni ifodalovchi enum. Uning asosiy variantlari `Ok(T)` va `Err(E)`.

## Why It Matters

Rust [[recoverable-errors|recoverable errors]]ni value sifatida ko'rsatadi. `read_line`, `parse`, `File::open`, va `read_to_string` kabi operations fail bo'lishi mumkin, shuning uchun ular `Result` qaytaradi.

## Mental Model

- `Ok(value)`: operation muvaffaqiyatli, value mavjud.
- `Err(error)`: operation muvaffaqiyatsiz, error information mavjud.

`T` success value type'i, `E` error value type'i. Masalan `File::open("hello.txt")` uchun `T` `std::fs::File`, `E` esa `std::io::Error`.

Chapter 10.1 `Result<T, E>`ni [[generic-enums|generic enum]] sifatida qayta ko'rsatadi: `Ok(T)` success branch payloadi, `Err(E)` error branch payloadi.

Fail bo'lishi mumkin bo'lgan function definitionida `Result` yaxshi default: caller recover qilishi, default tanlashi, retry qilishi, yoki o'zi panic qilishi mumkin.

Backend beginner source bu modelni chuqurlashtiradi: `Err(E)`dagi `E` oddiy string bo'lishi mumkin, lekin ko'pincha custom enum error type yaxshiroq signal beradi. `NameParseError` misoli errorni ham domain data sifatida ko'rsatadi.

O'sha source yana uchta amaliy qatlam qo'shadi: [[std-error-trait|std::error::Error]] ecosystem contracti, `map`/`and_then` combinatorlari, va `?` bilan linear [[error-propagation|error propagation]]. Oxirida ignored `Result` warning signalini ongli bosish uchun `let _ = ...;` patterni ko'rsatiladi.

## Syntax and Examples

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

File open:

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {error:?}"),
    };
}
```

Propagation with `?`:

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

Custom error enum:

```rust
#[derive(Debug)]
enum NameParseError {
    EmptyString,
    NoMiddleName,
}
```

Ignored `Result`:

```rust
let _ = function_that_may_fail();
```

## Common Mistakes

- `Result`ni e'tiborsiz qoldirish. Compiler `unused Result that must be used` warning beradi.
- Hamma joyda `.expect(...)` ishlatish. Beginner code uchun qulay, lekin user input kabi recoverable holatlarda `match` yaxshiroq.
- `Result`ni [[panic|panic!]] o'rniga ishlatiladigan recoverable path ekanini unutish.
- `?` errorni handle qiladi deb o'ylash; u [[error-propagation|propagate]] qiladi.
- `Ok(...)` bilan success value'ni o'rashni unutish.
- Callerga tanlov berish kerak bo'lgan API'da panic qilish.
- `Result`ni shunchaki string message transporti deb ko'rish.
- Ignored `Result` warningini shovqin deb qabul qilib, asl signalni yo'qotish.

## Related Concepts

- [[match]]
- [[generics]]
- [[generic-enums|generic enums]]
- [[generic-type-parameters|generic type parameters]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[error-propagation|error propagation]]
- [[question-mark-operator|question mark operator]]
- [[std-error-trait|std::error::Error]]
- [[unwrap]]
- [[expect]]
- [[io-error|io::Error]]
- [[panic|panic!]]
- [[panic-vs-result|panic! vs Result]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/sources/9-error-handling]]
- [[9-2-recoverable-errors-with-result]]
- [[9-3-to-panic-or-not-to-panic]]
- [[10-1-generic-data-types]]
- [[wiki/sources/rust-for-backend-developers-result]]
