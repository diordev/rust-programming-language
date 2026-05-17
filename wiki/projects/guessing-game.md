---
title: "Guessing Game"
type: project
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, project, cli]
source_count: 2
---

# Guessing Game

## Goal

CLI game: program 1 dan 100 gacha secret number yaratadi, userdan guess oladi, guess past/baland/to'g'ri ekanini aytadi, to'g'ri javobda tugaydi.

## Requirements

- Project [[cargo|Cargo]] bilan yaratiladi.
- Random number uchun [[rand]] crate ishlatiladi.
- User input terminaldan olinadi.
- Input `String`dan `u32`ga convert qilinadi.
- Non-number input programni crash qilmasligi kerak.
- Correct guess bo'lsa `break` bilan game tugaydi.

## Design Notes

- `let mut guess = String::new()` input buffer sifatida ishlaydi.
- `read_line(&mut guess)` inputni stringga append qiladi.
- `guess.trim().parse()` newline/whitespace olib tashlab numberga convert qiladi.
- `match` error handling uchun ishlatiladi: `Ok(num)` numberni qaytaradi, `Err(_)` invalid inputni ignore qilib `continue` qiladi.
- `guess.cmp(&secret_number)` `Ordering` qaytaradi.
- Chapter 9.3 range validationni kuchaytiradi: guess 1..=100 oralig'ida bo'lishi kerak.
- Simple loop ichida `if guess < 1 || guess > 100 { continue; }` check ishlatish mumkin.
- Invariant muhim bo'lsa, [[guess-validation-type|Guess validation type]] kabi custom type range checkni constructor ichiga ko'chiradi.

## Implementation Log

1. `cargo new guessing_game` bilan project yaratildi.
2. User input o'qish qo'shildi.
3. `rand = "0.8.5"` dependency sifatida qo'shildi.
4. Secret number `rand::thread_rng().gen_range(1..=100)` bilan yaratildi.
5. Guess stringdan `u32`ga convert qilindi.
6. `match` orqali too small / too big / win branchlari yozildi.
7. `loop` orqali multiple guesses qo'shildi.
8. Correct guess uchun `break`, invalid input uchun `continue` qo'shildi.

## Concepts Practiced

- [[cargo|Cargo]]
- [[crate]]
- [[rand]]
- [[variables-and-mutability|variables and mutability]]
- [[reference]]
- [[result|Result]]
- [[match]]
- [[loop]]
- [[shadowing]]
- [[panic-vs-result|panic! vs Result]]
- [[custom-validation-types|custom validation types]]
- [[invariants]]

## Code

```rust
use std::cmp::Ordering;
use std::io;

use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

## Open Questions

- Guess count qaysi type bilan saqlangani yaxshiroq?
- Userga `quit` command qo'shish uchun parse logic qanday o'zgaradi?
- `rand::thread_rng` o'rniga yangi `rand` APIsida qanday usul ishlatiladi?
- Range validation user-facing recoverable error bo'lishi kerakmi yoki `Guess::new` contract violation sifatida panic qilishi kerakmi?

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[9-3-to-panic-or-not-to-panic]]
