---
title: "Error Handling"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, error-handling]
source_count: 6
---

# Error Handling

## Short Definition

Rust error handlingni value va control flow orqali ko'rsatadi: [[recoverable-errors|recoverable errors]] odatda [[result|Result<T, E>]], [[unrecoverable-errors|unrecoverable errors]] esa [[panic|panic!]] bilan ifodalanadi.

## Why It Matters

Rustda compiler error casesni ochiq ko'rsatishga majbur qiladi. Error handling function signature, `match`, `Result`, yoki explicit panic orqali ko'rinadi; shu sabab fail bo'lishi mumkin bo'lgan pathlar productiondan oldin ko'rib chiqiladi.

## Mental Model

Error yashirin exception emas. Caller uchta asosiy qarordan birini qiladi:

- `Result<T, E>`ni handle qiladi.
- Errorni yuqoriga uzatadi.
- Holat unrecoverable bo'lsa `panic!` bilan executionni to'xtatadi.

Beginner mental model: user/environment xatolari ko'pincha recoverable; violated invariant, impossible state, yoki invalid internal operation ko'pincha unrecoverable.

Chapter 9.2dan keyingi practical rule: local context recovery qilishga yetarli bo'lsa `match` yoki helper method bilan handle qiling; caller yaxshiroq qaror qila olsa [[question-mark-operator|?]] bilan [[error-propagation|propagate]] qiling.

Backend beginner result source bu modelni uchta qo'shimcha signal bilan mustahkamlaydi: custom error enumlar stringdan yaxshiroq API signal bo'lishi mumkin, [[std-error-trait|std::error::Error]] ecosystem uchun umumiy contract beradi, va `?` linear code beradi, lekin errorni handle qilmaydi.

## Syntax and Examples

Recoverable parsing:

```rust
let number: u32 = match input.trim().parse() {
    Ok(value) => value,
    Err(_) => return,
};
```

Unrecoverable panic:

```rust
fn main() {
    panic!("crash and burn");
}
```

Propagate with `?`:

```rust
use std::fs;
use std::io;

fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

Ignored `Result`ni explicit discard:

```rust
let _ = function_that_may_fail();
```

## Common Mistakes

- Har bir `Result` uchun `.unwrap()` ishlatish.
- Recoverable user input errorini program crash bilan tugatish.
- `panic!`ni normal control flow o'rniga ishlatish.
- Panic va compile-time compiler errorni bir xil narsa deb o'ylash.
- `unwrap`ni error handling deb qabul qilish.
- `?` errorni avtomatik hal qiladi deb o'ylash.
- Custom error type kerak joyda hamma narsani stringga tushirib yuborish.

## Related Concepts

- [[recoverable-errors|recoverable errors]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[result|Result]]
- [[panic|panic!]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[error-propagation|error propagation]]
- [[question-mark-operator|question mark operator]]
- [[std-error-trait|std::error::Error]]
- [[unwrap]]
- [[expect]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/sources/0-2-introduction]]
- [[wiki/sources/9-error-handling]]
- [[9-1-unrecoverable-errors-with-panic]]
- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/rust-for-backend-developers-result]]
