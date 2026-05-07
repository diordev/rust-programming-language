---
title: "9.2. Recoverable Errors with Result - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, error-handling, result]
source_count: 1
source_path: "raw/books/the_rust_programming_language/9.2. Recoverable Errors with Result - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch09-02-recoverable-errors-with-result.html"
---

# 9.2. Recoverable Errors with Result - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/9.2. Recoverable Errors with Result - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch09-02-recoverable-errors-with-result.html
- Date processed: 2026-05-07

## Detailed Summary

Section 9.2 [[recoverable-errors|recoverable errors]]ni [[result|Result<T, E>]] orqali handle qilishni chuqurlashtiradi. Ko'p xatolar programni butunlay to'xtatishni talab qilmaydi. Masalan file ochish fail bo'lsa, file yo'q bo'lishi mumkin; bunday holatda program file yaratib davom etishi mumkin.

`Result<T, E>` generic enum sifatida ikki variantdan iborat:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`T` success case'dagi value type'i, `E` failure case'dagi error type'i. Shu sabab `Result` turli contextlarda ishlaydi: success value va error value typelari har xil bo'lishi mumkin. Masalan `File::open("hello.txt")` return type'i `Result<std::fs::File, std::io::Error>`: success bo'lsa [[file-handle|file handle]], failure bo'lsa [[io-error|io::Error]] qaytadi.

Basic handling `match` orqali qilinadi:

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

`Result` va uning `Ok`/`Err` variants prelude'da bo'lgani uchun `Result::Ok` yoki `Result::Err` prefix shart emas. `Ok(file)` branch file handle'ni ochadi; `Err(error)` branch esa bu example'da [[panic|panic!]] qiladi.

Keyingi muhim fikr: hamma `Err` bir xil emas. `File::open` fail bo'lishi file yo'qligi, permission yo'qligi, yoki boshqa I/O sabablar bilan bo'lishi mumkin. `std::io::Error` ustidagi `kind()` method [[error-kind|ErrorKind]] enumini beradi. `ErrorKind::NotFound` bo'lsa `File::create("hello.txt")` orqali recovery qilish mumkin; boshqa errorlarda panic qilish tanlanadi.

Nested `match` verbose bo'lishi mumkin. `Result<T, E>` helper methods bilan concise handling beradi. `unwrap_or_else` closure bilan error case'da logic bajaradi. Chapter closures'ni keyinroq chuqurlashtiradi, lekin bu method nested `match`ni qisqartirishi ko'rsatiladi.

[[unwrap]] va [[expect]] panic-on-error shortcutlari sifatida beriladi. `unwrap()` `Ok` bo'lsa inner value'ni qaytaradi, `Err` bo'lsa panic qiladi. `expect("message")` ham shunday, lekin panic message'ga context qo'shadi. Production-quality Rust code'da `unwrap`dan ko'ra `expect` afzalroq, chunki assumption buzilganda debugging uchun yaxshiroq message beradi.

[[error-propagation|Error propagation]] sectioni function ichida errorni handle qilmasdan callerga qaytarish patternini ko'rsatadi. `read_username_from_file() -> Result<String, io::Error>` success bo'lsa `Ok(username)` qaytaradi; `File::open` yoki `read_to_string` fail bo'lsa `Err(io::Error)` callerga qaytadi. Bu callerga ko'proq context asosida qaror qilish imkonini beradi: panic qilish, default username ishlatish, boshqa source'dan o'qish va hokazo.

[[question-mark-operator|Question mark operator]] `?` error propagation boilerplate'ini qisqartiradi. `File::open("hello.txt")?` `Ok(file)` bo'lsa file'ni expression value sifatida beradi; `Err(e)` bo'lsa butun functiondan early return qilib errorni callerga uzatadi. `?` errorni current function return type'idagi errorga convert qilish uchun [[from-trait|From trait]]dagi `from` functiondan foydalanishi mumkin.

Example qisqarish bosqichlari:

```rust
let mut username_file = File::open("hello.txt")?;
let mut username = String::new();
username_file.read_to_string(&mut username)?;
Ok(username)
```

Keyin method chaining:

```rust
File::open("hello.txt")?.read_to_string(&mut username)?;
```

Eng qisqa version esa standard librarydagi `fs::read_to_string("hello.txt")` bo'lib, file ochish, `String` yaratish, o'qish, va `Result` qaytarishni bir functionga jamlaydi.

`?` operator faqat compatible return type bo'lgan functionda ishlaydi. `fn main() { File::open("hello.txt")?; }` compile bo'lmaydi, chunki `main` default `()` qaytaradi, lekin `?` `Err`ni qaytarishi uchun `Result` yoki `Option` compatible return type kerak. Bu holatda compiler [[e0277-trait-bound-not-satisfied|E0277]] beradi va `fn main() -> Result<(), Box<dyn std::error::Error>>` kabi signature tavsiya qiladi.

`?` `Option<T>` bilan ham ishlaydi: function `Option` qaytarsa, `None` early return bo'ladi, `Some(value)` esa value'ni ochadi. `Result` va `Option` avtomatik aralashmaydi; conversion kerak bo'lsa `ok` yoki `ok_or` kabi methods ishlatiladi.

`main` special function bo'lsa ham, `Result<(), E>` qaytarishi mumkin. `Ok(())` program successful exit code `0` beradi, `Err` esa nonzero exit statusga olib keladi. `Box<dyn Error>` hozircha "any kind of error" deb o'qilishi mumkin; trait objects keyinroq Chapter 18da chuqurlashadi.

## Key Concepts

- [[recoverable-errors|recoverable errors]]
- [[result|Result<T, E>]]
- [[file-handle|file handle]]
- [[io-error|io::Error]]
- [[error-kind|ErrorKind]]
- [[match]]
- [[unwrap]]
- [[expect]]
- [[error-propagation|error propagation]]
- [[question-mark-operator|question mark operator]]
- [[from-trait|From trait]]
- [[option|Option]]
- [[main-function|main function]]
- [[box-dyn-error|Box<dyn Error>]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Code Examples

Open a file:

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
}
```

Handle `Result` with `match`:

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

Handle `NotFound` differently:

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {e:?}"),
            },
            _ => panic!("Problem opening the file: {error:?}"),
        },
    };
}
```

`expect` with context:

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")
        .expect("hello.txt should be included in this project");
}
```

Manual error propagation:

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
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

Short standard library version:

```rust
use std::fs;
use std::io;

fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

`?` with `Option`:

```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

`main` returning `Result`:

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

## Exercises or Practice Ideas

- `File::open("hello.txt")` return type'ini `Result<T, E>`dagi `T` va `E` bilan yozing.
- `match` yordamida file not found bo'lsa create qiladigan, permission error bo'lsa panic qiladigan code yozing.
- `unwrap` va `expect`ni bir xil fail holatda sinab, panic message farqini yozing.
- Manual `return Err(e)` bilan yozilgan functionni `?` operator bilan qayta yozing.
- `?` nima uchun `fn main() { ... }` ichida default ishlamasligini return type bilan tushuntiring.
- `Option` qaytaradigan functionda `?` ishlatib, `None` early return bo'lishini ko'rsating.

## Questions Raised

- Recoverable errorni qachon local handle qilish kerak, qachon callerga propagate qilish kerak?
- `expect` message qanday yozilsa debuggingda foydali bo'ladi?
- `?` operator error type conversionni qanday qilib `From` trait orqali bajaradi?
- `Result` va `Option` orasidagi explicit conversion qachon kerak bo'ladi?
- `main -> Result<(), E>` executable exit status bilan qanday bog'lanadi?

## Links To Update

- [[9-error-handling|9. Error Handling]]
- [[recoverable-errors|recoverable errors]]
- [[result|Result<T, E>]]
- [[error-propagation|error propagation]]
- [[question-mark-operator|question mark operator]]
- [[unwrap]]
- [[expect]]
- [[error-kind|ErrorKind]]
- [[io-error|io::Error]]
- [[file-handle|file handle]]
- [[from-trait|From trait]]
- [[main-function|main function]]
- [[box-dyn-error|Box<dyn Error>]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
