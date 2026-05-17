---
title: "2. Programming a Guessing Game - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, project]
source_count: 1
source_path: "raw/books/the_rust_programming_language/2. Programming a Guessing Game.md"
source_url: "https://doc.rust-lang.org/stable/book/ch02-00-guessing-game-tutorial.html"
---

# 2. Programming a Guessing Game - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/2. Programming a Guessing Game.md`
- Type: book project chapter
- URL: https://doc.rust-lang.org/stable/book/ch02-00-guessing-game-tutorial.html
- Date processed: 2026-05-06

## Detailed Summary

Bu chapter Rustga hands-on project orqali kiradi: [[guessing-game]] programi. Program 1 dan 100 gacha random secret number yaratadi, userdan guess so'raydi, guess juda kichik yoki katta ekanini aytadi, to'g'ri guess bo'lsa yutganini chiqarib tugaydi.

Chapter avval [[cargo|Cargo]] project yaratishdan boshlaydi:

```shell
$ cargo new guessing_game
$ cd guessing_game
$ cargo run
```

Generated `Cargo.toml` `name`, `version`, `edition = "2024"` va `[dependencies]` sectionlarini beradi. `src/main.rs` default `Hello, world!` programdan boshlanadi. Chapterning qolgan qismida hamma code shu `src/main.rs` ichida yoziladi.

User input uchun `use std::io;` bilan standard librarydagi `io` module scopega olib kiriladi. Rust prelude har bir programga ko'p common itemlarni avtomatik olib kiradi, lekin `std::io` prelude ichida emas, shuning uchun explicit `use` kerak.

Input saqlash uchun:

```rust
let mut guess = String::new();
```

`let` variable binding yaratadi. Rust variables default immutable. `mut` variable qiymatini o'zgartirishga ruxsat beradi. `String::new()` growable, UTF-8 encoded, empty `String` yaratadi. `::new` bu `String` type ustidagi associated function.

User input `io::stdin().read_line(&mut guess)` orqali olinadi. `read_line` user typed qilgan textni berilgan `String` oxiriga append qiladi, shu sababli mutable reference kerak: `&mut guess`. `&` reference ekanini bildiradi; references default immutable bo'ladi, shuning uchun `&guess` emas `&mut guess` yoziladi.

`read_line` `Result` qaytaradi. `Result` enum bo'lib, `Ok` va `Err` variantlariga ega. `Ok` success value saqlaydi, `Err` failure haqida information saqlaydi. Bu chapterda birinchi bosqichda `.expect("Failed to read line")` ishlatiladi: `Err` bo'lsa program panic qiladi, `Ok` bo'lsa success value qaytadi. Agar `Result` ishlatilmasa, compiler `unused Result that must be used` warning beradi, chunki error handling e'tiborsiz qoldirilgan bo'ladi.

`println!` placeholders ham ko'rsatiladi. Variable qiymatini format string ichida `{guess}` bilan chiqarish mumkin. Expression natijasini chiqarish uchun `{}` placeholder va keyin argument list ishlatiladi:

```rust
println!("x = {x} and y + 2 = {}", y + 2);
```

Secret number yaratish uchun standard libraryda random functionality yo'qligi aytiladi; buning uchun [[rand]] crate ishlatiladi. `Cargo.toml`ga dependency qo'shiladi:

```toml
[dependencies]
rand = "0.8.5"
```

Cargo external crate dependencylarini Crates.io registrydan oladi, dependency tree ichidagi kerakli crate'larni ham download/build qiladi. Version specifier `0.8.5` SemVer bo'yicha `^0.8.5` degani: kamida `0.8.5`, lekin `0.9.0`dan past compatible versionlar. `Cargo.lock` birinchi buildda exact dependency versionsni yozib, reproducible builds beradi. `cargo update` `Cargo.lock`ni ignore qilib, `Cargo.toml`dagi constraintlarga mos yangi versionlarni tanlaydi.

Random number uchun:

```rust
use rand::Rng;

let secret_number = rand::thread_rng().gen_range(1..=100);
```

`Rng` trait random number generatorlar implement qiladigan methodsni beradi; `gen_range` method ishlashi uchun trait scope ichida bo'lishi kerak. `1..=100` inclusive range expression: pastki va yuqori bound ham kiradi.

Guess bilan secret number solishtirish uchun `std::cmp::Ordering` ishlatiladi. `Ordering` enum variantlari: `Less`, `Greater`, `Equal`. `cmp` method ikki comparable valueni solishtirib `Ordering` qaytaradi. `match` expression `Ordering` variantiga qarab branch tanlaydi:

```rust
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

Bir intermediate listing compile bo'lmaydi va `error[E0308]: mismatched types` beradi, chunki `guess` hali `String`, `secret_number` esa number. Rust strong, static type systemga ega, lekin type inference ham ishlaydi. `secret_number` default `i32` deb inferred bo'lishi mumkin; keyingi code `guess: u32` annotation orqali `secret_number`ni ham `u32`ga infer qilishga olib keladi.

String inputni numberga o'tkazish:

```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

Bu yerda [[shadowing]] ishlatiladi: oldingi `guess` (`String`) nomi yangi `guess` (`u32`) bilan qayta ishlatiladi. `trim()` newline yoki Windowsdagi `\r\n`ni olib tashlaydi. `parse()` stringni requested typega convert qiladi; target type `let guess: u32` annotationdan keladi. `parse()` ham `Result` qaytaradi, shuning uchun oldin `.expect(...)` bilan crash-on-error ishlatiladi.

Multiple guesses uchun `loop` ishlatiladi. `loop` infinite loop yaratadi; prompt, input, parse va compare logic loop ichiga ko'chiriladi. To'g'ri guess bo'lsa `Ordering::Equal` arm ichida `break` bilan loopdan chiqiladi.

Invalid input handling yakuniy refinement: `.expect("Please type a number!")` o'rniga `match` ishlatiladi:

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

`Ok(num)` successful parsed numberni oladi. `Err(_)` parse errorning har qanday detailini ignore qiladi va `continue` orqali loopning keyingi iterationiga o'tadi. Shunday qilib program non-number inputda crash qilmaydi, balki yana guess so'raydi.

Final version debug uchun chiqarilgan `println!("The secret number is: {secret_number}")` line'ni olib tashlaydi. Chapter yakunida keyingi chapterlar bu chapterda ko'rilgan conceptsni chuqurroq tushuntirishi aytiladi: variables, data types, functions, [[ownership]], structs/method syntax, enums.

## Key Concepts

- [[guessing-game]]
- [[cargo|Cargo]]
- [[variables-and-mutability|variables and mutability]]
- [[shadowing]]
- [[result|Result]]
- [[match]]
- [[loop]]
- [[reference]]
- [[mutable-reference|mutable reference]]
- [[crate]]
- [[rand]]
- [[semver|SemVer]]
- [[cargo-lock|Cargo.lock]]
- [[ordering|Ordering]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Code Examples

Final guessing game:

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

Dependency:

```toml
[dependencies]
rand = "0.8.5"
```

## Exercises or Practice Ideas

- Final code'ni qo'lda yozib `cargo run` bilan sinash.
- `secret_number`ni vaqtincha print qilib, compare logicni test qilish; keyin printni olib tashlash.
- `Err(_) => continue` o'rniga error message chiqarib keyin `continue` qilish.
- Guess count qo'shish: user nechta urinishda topganini chiqarish.
- Range'ni `1..=1000`ga o'zgartirish va promptni moslash.

## Questions Raised

- `expect` qachon yetarli, qachon `match` bilan recoverable error handling kerak?
- `String`dan `u32`ga o'tishda [[shadowing]] nima uchun qulay?
- `Cargo.lock` application va library crate'larda qanday boshqariladi?
- `Rng` trait scopega olib kirilmasa, `gen_range` nima uchun topilmaydi?
- `loop`, `break`, `continue` kombinatsiyasi real input workflowlarda qanday ishlatiladi?

## Links To Update

- [[wiki/chapters/2-programming-a-guessing-game|2. Programming a Guessing Game]]
- [[guessing-game]]
- [[rand]]
- [[e0308-mismatched-types|E0308 mismatched types]]
- [[cargo|Cargo]]
- [[rust-learning-roadmap]]
