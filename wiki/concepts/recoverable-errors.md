---
title: "Recoverable Errors"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling]
source_count: 4
---

# Recoverable Errors

## Short Definition

Recoverable errors - program davom etishi, userga xabar berishi, retry qilishi, yoki callerga error qaytarishi mumkin bo'lgan xatolar.

## Why It Matters

Rust recoverable errorlarni yashirin exception qilib yubormaydi. Ular ko'pincha [[result|Result<T, E>]] type'i orqali function signature'da ko'rinadi va caller bu holatni handle qilishi kerak.

## Mental Model

Recoverable error "bu operation fail bo'lishi mumkin, lekin programning butun modeli buzilmadi" degani. File topilmasligi, invalid user input, parsing xatosi, yoki network failure kabi holatlar odatda recovery path talab qiladi.

Recovery local bo'lishi mumkin: `ErrorKind::NotFound` bo'lsa file yaratish. Yoki error callerga [[error-propagation|propagate]] qilinishi mumkin.

Expected failures recoverable bo'lishi kerak: malformed parser input, HTTP rate limit, user wrong format kabi holatlar normal hayotda uchraydi va `Result` orqali callerga qaytarilishi kerak.

## Syntax and Examples

```rust
let number: u32 = match input.trim().parse() {
    Ok(value) => value,
    Err(_) => return,
};
```

`Err(_)` branch panic emas; u errorni tan olib, program flowini boshqaradi.

File missing bo'lsa recovery:

```rust
use std::fs::File;
use std::io::ErrorKind;

let greeting_file = match File::open("hello.txt") {
    Ok(file) => file,
    Err(error) => match error.kind() {
        ErrorKind::NotFound => File::create("hello.txt").unwrap(),
        _ => panic!("Problem opening the file: {error:?}"),
    },
};
```

## Common Mistakes

- User input xatosini [[panic|panic!]] bilan tugatish.
- Har bir `Result` uchun `.unwrap()` ishlatib, recoverable holatni unrecoverable qilib yuborish.
- Error handlingni "keyin qo'shaman" deb API design'dan yashirish.
- Barcha `Err` holatlarini bir xil deb ko'rish.
- Caller yaxshiroq qaror qila oladigan errorni local panic bilan tugatish.
- Expected failure'ni caller-side bug deb belgilash.

## Related Concepts

- [[error-handling]]
- [[result|Result<T, E>]]
- [[match]]
- [[error-kind|ErrorKind]]
- [[error-propagation|error propagation]]
- [[question-mark-operator|question mark operator]]
- [[panic-vs-result|panic! vs Result]]
- [[unrecoverable-errors|unrecoverable errors]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[9-error-handling-the-rust-programming-language]]
- [[9-2-recoverable-errors-with-result-the-rust-programming-language]]
- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
