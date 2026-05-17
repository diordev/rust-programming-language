---
title: "std::io::Result type alias"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, types, alias, error-handling, stdlib]
source_count: 1
---

# `std::io::Result` Type Alias

## Avval — Takror Yozish

`std::io` moduli ichidagi `Write` trait'i `Result<T, std::io::Error>` ko'p marta yozadi:

```rust
use std::fmt;
use std::io::Error;

pub trait Write {
    fn write(&mut self, buf: &[u8]) -> Result<usize, Error>;
    fn flush(&mut self) -> Result<(), Error>;
    fn write_all(&mut self, buf: &[u8]) -> Result<(), Error>;
    fn write_fmt(&mut self, fmt: fmt::Arguments) -> Result<(), Error>;
}
```

Har funksiya signaturada `Result<..., Error>` qaytariladi. Takrorlanish.

## Type Alias Bilan

`std::io` modulida shunday alias bor:

```rust
type Result<T> = std::result::Result<T, std::io::Error>;
```

Endi `Write` trait'i:

```rust
pub trait Write {
    fn write(&mut self, buf: &[u8]) -> Result<usize>;
    fn flush(&mut self) -> Result<()>;
    fn write_all(&mut self, buf: &[u8]) -> Result<()>;
    fn write_fmt(&mut self, fmt: fmt::Arguments) -> Result<()>;
}
```

`Result<T, std::io::Error>` o'rniga `Result<T>` — qisqaroq, bir xil interfeys.

## Foydalanish

```rust
use std::io::{self, Write};

fn write_hello(w: &mut impl Write) -> io::Result<()> {
    w.write_all(b"hello\n")
}
```

`io::Result<()>` — `std::io::Result<()>` — `std::result::Result<(), std::io::Error>`.

## Type Alias `Result<T, E>` Method'lariga Ta'sir Qilmaydi

Alias yangi tip emas — barcha `Result<T, E>` method'lari ishlaydi:

```rust
fn main() -> io::Result<()> {
    let n = std::fs::read("config.toml")?;        // ? operator OK
    let _: Vec<u8> = n.iter().map(|b| *b + 1).collect();
    Ok(())
}
```

`?` operator, `map`, `unwrap_or`, `is_err`, `and_then` — barchasi avtomatik.

## Boshqa Standart Kutubxona Aliaslari

```rust
type Result<T> = std::result::Result<T, std::io::Error>;       // std::io
type Result<T> = std::result::Result<T, std::fmt::Error>;      // std::fmt
type Cow<'a, B> = std::borrow::Cow<'a, B>;                     // re-export
```

Har modul o'z `Result<T>` aliasini ishlatadi (`E` parametrini fix qilib).

## Foyda

1. **Yozish qisqaroq.** `Result<T>` 13 ta belgi, `Result<T, std::io::Error>` 30+.
2. **Konsistent interfeys.** Modul ichidagi barcha funksiyalar bir xil shaklda.
3. **Refactor osonroq.** Agar `std::io::Error` ni `MyError` ga o'zgartirmoqchi bo'lsangiz — bitta alias deklaratsiyasi.
4. **`?` saqlanadi.** Alias type emas, oddiy `Result<T, E>` operatorlari ishlaydi.

## Related

- [[type-alias|type alias]]
- [[result|Result]]
- [[io-error|io::Error]]
- [[error-handling]]
- [[question-mark-operator|? operator]]
- [[error-propagation|error propagation]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
