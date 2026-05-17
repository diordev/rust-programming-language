---
title: "9.3. To panic! or Not to panic! - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, error-handling, panic, result]
source_count: 1
source_path: "raw/books/the_rust_programming_language/9.3. To panic! or Not to panic!.md"
source_url: "https://doc.rust-lang.org/stable/book/ch09-03-to-panic-or-not-to-panic.html"
---

# 9.3. To panic! or Not to panic! - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/9.3. To panic! or Not to panic!.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch09-03-to-panic-or-not-to-panic.html
- Date processed: 2026-05-07

## Detailed Summary

Section 9.3 [[panic-vs-result|panic! vs Result]] qarorini tushuntiradi. `panic!` bo'lsa recovery yo'q: program current pathda davom eta olmaydi. Har qanday error uchun `panic!` chaqirish mumkin, lekin bu caller nomidan "bu holat unrecoverable" degan qarorni qabul qilishdir. Function `Result` qaytarsa, callerga tanlov beradi: recover qilish, default ishlatish, retry qilish, yoki o'zi `panic!` qilib recoverable errorni unrecoverable errorga aylantirish. Shu sabab fail bo'lishi mumkin bo'lgan functionlar uchun [[result|Result<T, E>]] yaxshi default hisoblanadi.

`panic!` ba'zi contextlarda o'rinli. Examples, prototype code, va testsda [[unwrap]] yoki [[expect]] ishlatish acceptable bo'lishi mumkin. Example code'ga to'liq robust error handling qo'shish asosiy conceptni xiralashtirishi mumkin. Prototype paytida `unwrap`/`expect` keyinroq robust error handling qo'shilishi kerak bo'lgan joylarni marker qilib turadi. Testsda esa fail bo'lgan helper call butun testni fail qilishi kerak; Rust test failure uchun panicdan foydalanadi.

Developer compilerdan ko'proq ma'lumotga ega bo'lgan holatda `expect` ishlatishi ham mos. Masalan hardcoded `"127.0.0.1"` valid IP address ekanini developer biladi, lekin `parse()` umumiy API bo'lgani uchun baribir `Result` qaytaradi. Bu holatda:

```rust
let home: IpAddr = "127.0.0.1"
    .parse()
    .expect("Hardcoded IP address should be valid");
```

`expect` message assumptionni yozib qo'yadi. Agar keyin input hardcoded stringdan user inputga o'zgarsa, message developerga bu joyda real error handling kerakligini eslatadi.

Guidelines section [[bad-state|bad state]] tushunchasini beradi. Bad state - assumption, guarantee, [[function-contracts|contract]], yoki [[invariants|invariant]] buzilgan holat. Panic ayniqsa quyidagi shartlar birga bo'lsa o'rinli: holat unexpected bo'lsa, keyingi code bad state yo'qligiga tayanishi kerak bo'lsa, va bu constraintni typelar orqali encode qilishning yaxshi yo'li bo'lmasa.

Library/API code'da invalid input olinsa, agar caller recover qila olsa `Result` qaytarish yaxshi. Lekin continuing insecure yoki harmful bo'lsa, `panic!` caller code'idagi bugni development paytida ochib beradi. Standard library out-of-bounds memory accessda panic qiladi, chunki invalid memory access security problem bo'lishi mumkin. Functionlar contractga ega: behavior faqat inputlar talablarni bajarsa guaranteed. Contract violation caller-side bug bo'lgani uchun panic mantiqli bo'lishi mumkin, lekin bunday panic public API documentationda tushuntirilishi kerak.

Expected failures uchun `Result` yaxshiroq: malformed parser input, HTTP rate limit, missing external resource kabi holatlar normal dunyo sharoitida vaqti-vaqti bilan bo'lishi mumkin. `Result` callerga bu expected possibility ekanini bildiradi.

Section Rust type systemdan validation uchun foydalanishni tavsiya qiladi. Agar function parameter type'i `Option<T>` emas, `T` bo'lsa, compiler value borligini tekshirgan bo'ladi; function `None` case'ni runtime tekshirmaydi. `u32` negative value kiritilmasligini type orqali kafolatlaydi.

Custom type validation section guessing game example'i orqali [[custom-validation-types|custom validation types]] patternini ko'rsatadi. `Guess` type faqat 1..=100 oralig'idagi value bilan yaratilishi kerak. `Guess::new(value)` value range'ni tekshiradi; range buzilsa panic qiladi, chunki invalid `Guess` yaratish API contract violation. `value` field private qilingan, shuning uchun outside code `Guess { value: 200 }` qila olmaydi va constructorni chetlab o'tmaydi. Public getter `value(&self) -> i32` safe access beradi. Shundan keyin function signature `i32` o'rniga `Guess` qabul qilsa, body ichida range checkni qaytarib yozish shart emas.

Chapter 9 summary: `panic!` program handle qila olmaydigan state'ni signal qiladi; `Result` recoverable failure possibility'ni type system orqali callerga bildiradi. Ikkalasini joyida ishlatish Rust code'ni reliable qiladi. Keyingi chapter genericsga o'tadi, chunki `Option<T>` va `Result<T, E>` standard library genericsning foydali examplesidir.

## Key Concepts

- [[panic-vs-result|panic! vs Result]]
- [[panic|panic!]]
- [[result|Result<T, E>]]
- [[unwrap]]
- [[expect]]
- [[bad-state|bad state]]
- [[invariants]]
- [[function-contracts|function contracts]]
- [[api-design|API design]]
- [[custom-validation-types|custom validation types]]
- [[guess-validation-type|Guess validation type]]
- [[type-checking|type checking]]
- [[option|Option]]
- [[testing]]

## Code Examples

`expect` when developer knows more than the compiler:

```rust
fn main() {
    use std::net::IpAddr;

    let home: IpAddr = "127.0.0.1"
        .parse()
        .expect("Hardcoded IP address should be valid");
}
```

Manual range check in guessing game:

```rust
let guess: i32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};

if guess < 1 || guess > 100 {
    println!("The secret number will be between 1 and 100.");
    continue;
}
```

Validated `Guess` type:

```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {value}.");
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
```

## Exercises or Practice Ideas

- Har bir holat uchun `panic!` yoki `Result` tanlang: malformed user input, violated internal invariant, HTTP rate limit, hardcoded valid IP parse, out-of-bounds memory access.
- `expect` message'ini "nima uchun success bo'lishi kerak?" shaklida yozishga mashq qiling.
- Guessing game'da range checkni loop ichida qilish va `Guess` custom type orqali qilishni solishtiring.
- `Guess` field'ini public qilib ko'ring va invariant qanday buzilishini tushuntiring.
- Function contract violation bo'lsa public API documentationda qaysi panic condition yozilishi kerakligini ro'yxat qiling.

## Questions Raised

- Library function invalid input olsa, qachon callerga `Result` qaytarishi kerak, qachon panic qilishi kerak?
- "Expected failure" va "caller-side bug" orasidagi amaliy farq qanday aniqlanadi?
- `expect` message qachon haqiqatan useful documentation hisoblanadi?
- Type system orqali validation qilish runtime checksni qayerda kamaytiradi?
- Custom validation type panic qilishi kerakmi yoki `Result` qaytarishi kerakmi? Bu API contractga qanday bog'liq?

## Links To Update

- [[wiki/chapters/9-error-handling|9. Error Handling]]
- [[panic-vs-result|panic! vs Result]]
- [[panic|panic!]]
- [[result|Result<T, E>]]
- [[unwrap]]
- [[expect]]
- [[bad-state|bad state]]
- [[invariants]]
- [[function-contracts|function contracts]]
- [[custom-validation-types|custom validation types]]
- [[guess-validation-type|Guess validation type]]
- [[guessing-game|Guessing Game]]
