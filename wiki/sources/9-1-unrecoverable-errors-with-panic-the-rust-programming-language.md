---
title: "9.1. Unrecoverable Errors with panic! - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, error-handling, panic]
source_count: 1
source_path: "raw/books/the_rust_programming_language/9.1. Unrecoverable Errors with panic! - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch09-01-unrecoverable-errors-with-panic.html"
---

# 9.1. Unrecoverable Errors with panic! - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/9.1. Unrecoverable Errors with panic! - The Rust Programming Language.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch09-01-unrecoverable-errors-with-panic.html
- Date processed: 2026-05-07

## Detailed Summary

Section 9.1 [[panic|panic!]] macro orqali [[unrecoverable-errors|unrecoverable errors]]ni ko'rsatadi. Panic ikki yo'l bilan yuzaga keladi: code bevosita `panic!` chaqiradi yoki invalid operation library ichida panicga olib keladi. Masalan `Vec<T>` ichida mavjud bo'lmagan indexga `[]` bilan murojaat qilish panic qiladi.

Default behavior: panic message chiqariladi, [[stack-unwinding|stack unwinding]] boshlanadi, functions stack bo'ylab ortga yurilib data cleanup qilinadi, keyin program tugaydi. Bu cleanup ishini kamaytirish va binary hajmini kichraytirish kerak bo'lsa, [[cargo-toml|Cargo.toml]] ichida release profile uchun `panic = 'abort'` yozish mumkin. [[abort-on-panic|Abort on panic]] stackni tozalab yurmaydi; program darhol tugaydi va ishlatilgan memoryni operating system tozalaydi.

Oddiy explicit panic example:

```rust
fn main() {
    panic!("crash and burn");
}
```

`cargo run` outputida panic message, source location (`src/main.rs:2:5` kabi), va kerak bo'lsa `RUST_BACKTRACE=1` ishlatish haqida note chiqadi. Agar panic bevosita user code ichida chaqirilgan bo'lsa, location shu macro call turgan line'ni ko'rsatadi. Agar panic library code ichida bo'lsa, error message librarydagi panic joyini ko'rsatishi mumkin; haqiqiy sababni topish uchun [[backtrace]] kerak bo'ladi.

Section vector out-of-bounds example orqali Rustning safety qarorini ko'rsatadi:

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

Index 99 mavjud emas, shuning uchun Rust panic qiladi. `[]` syntax element qaytarishi kerak; invalid index uchun to'g'ri qaytariladigan element yo'q. C tilida bunday out-of-bounds read undefined behaviorga, [[buffer-overread|buffer overread]]ga, va security vulnerabilityga olib kelishi mumkin. Rust esa invalid memory o'qishni davom ettirish o'rniga executionni to'xtatadi.

`RUST_BACKTRACE=1 cargo run` backtrace chiqaradi. [[backtrace]] - shu nuqtaga kelguncha chaqirilgan functions ro'yxati. Uni o'qishda yuqoridan pastga yurib, birinchi o'zingiz yozgan file line'ini topish kerak; o'sha joy odatda problem originated bo'lgan nuqta. Backtrace to'liq foydali bo'lishi uchun debug symbols kerak; `cargo build` yoki `cargo run` default dev profile'da debug symbols bilan build qiladi, `--release` esa boshqa profile.

Section yakunida panicni fix qilishning real yo'li "panicni yashirish" emas, balki code qaysi value bilan qaysi invalid actionni qilayotganini topish va o'sha behaviorni to'g'rilash ekanini ta'kidlaydi. Agar user-provided index bo'lsa, `v.get(index)` kabi `Option` qaytaradigan API panic qiladigan indexingdan yaxshiroq.

## Key Concepts

- [[panic|panic!]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[stack-unwinding|stack unwinding]]
- [[abort-on-panic|abort on panic]]
- [[backtrace]]
- [[vector-indexing|vector indexing]]
- [[buffer-overread|buffer overread]]
- [[cargo-toml|Cargo.toml]]
- [[release-build|release build]]
- [[option|Option]]
- [[result|Result<T, E>]]

## Code Examples

Explicit panic:

```rust
fn main() {
    panic!("crash and burn");
}
```

Abort on panic in release profile:

```toml
[profile.release]
panic = 'abort'
```

Out-of-bounds vector indexing:

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

Backtrace bilan run qilish:

```shell
$ RUST_BACKTRACE=1 cargo run
```

Recoverable access alternative:

```rust
let v = vec![1, 2, 3];

match v.get(99) {
    Some(value) => println!("{value}"),
    None => println!("No value at that index"),
}
```

## Exercises or Practice Ideas

- `panic!("crash and burn")` exampleini run qilib, source locationni outputdan toping.
- `v[99]` va `v.get(99)` behaviorini solishtiring: biri panic, ikkinchisi `None`.
- `RUST_BACKTRACE=1 cargo run` bilan backtrace chiqarib, birinchi o'zingiz yozgan source line'ni belgilang.
- `Cargo.toml`ga `[profile.release] panic = 'abort'` qo'shilsa, panic response mental modeli qanday o'zgarishini tushuntiring.
- User inputdan kelgan index uchun panic qiladigan `[]` o'rniga `get` ishlatadigan code yozing.

## Questions Raised

- Panic message library file'ini ko'rsatsa, user code'dagi haqiqiy sababni qanday topamiz?
- `panic = 'abort'` qachon foydali, qachon cleanup/unwinding afzal?
- Rustning out-of-bounds panic behaviori memory safety va securityga qanday xizmat qiladi?
- `panic!`ni test/prototype code'da ishlatish bilan production error handling o'rtasidagi farq nima?

## Links To Update

- [[9-error-handling|9. Error Handling]]
- [[panic|panic!]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[stack-unwinding|stack unwinding]]
- [[abort-on-panic|abort on panic]]
- [[backtrace]]
- [[vector-indexing|vector indexing]]
- [[buffer-overread|buffer overread]]
- [[cargo-toml|Cargo.toml]]
- [[release-build|release build]]
