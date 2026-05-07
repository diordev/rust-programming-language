---
title: "9. Error Handling"
type: chapter
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, chapter, error-handling]
source_count: 4
---

# 9. Error Handling

## Learning Goal

Rustda [[error-handling|error handling]]ni explicit control flow sifatida tushunish: [[recoverable-errors|recoverable errors]] uchun [[result|Result<T, E>]], [[unrecoverable-errors|unrecoverable errors]] uchun [[panic|panic!]], panic paytida [[stack-unwinding|unwinding]] yoki [[abort-on-panic|aborting]], [[backtrace]] yordamida panic sababini topish, va [[question-mark-operator|? operator]] orqali errorlarni callerga propagate qilish.

## Main Ideas

- Rust exceptions ishlatmaydi; error handling API va control flowda ko'rinadi.
- [[recoverable-errors|Recoverable errors]] program davom etishi mumkin bo'lgan holatlar; ular odatda `Result<T, E>` bilan qaytariladi.
- [[unrecoverable-errors|Unrecoverable errors]] odatda bug belgisi; ular `panic!` bilan executionni to'xtatadi.
- `panic!` bevosita chaqirilishi yoki invalid operation orqali yuzaga kelishi mumkin.
- Default panic [[stack-unwinding|stack unwinding]] qiladi: stack bo'ylab ortga yurib cleanup bajaradi.
- `panic = 'abort'` [[cargo-toml|Cargo.toml]] profile settingi stack cleanup o'rniga darhol abort qilishga o'tkazadi.
- Out-of-bounds `v[99]` panic qiladi, chunki `[]` syntax element qaytarishi kerak va invalid index uchun to'g'ri element yo'q.
- Rust out-of-bounds readni undefined behaviorga aylantirmaydi; panic orqali [[buffer-overread|buffer overread]] kabi security risklarni to'xtatadi.
- `RUST_BACKTRACE=1` [[backtrace]] chiqaradi; problemni topishda yuqoridan boshlab birinchi o'zingiz yozgan file line'ini qidirish kerak.
- Debug symbols dev profile'da default yoqilgan; `cargo run` va `cargo build` without `--release` backtrace uchun ko'proq ma'lumot beradi.
- 9.2 [[result|Result<T, E>]]ni recoverable success/failure enum sifatida chuqurlashtiradi: `Ok(T)` success value, `Err(E)` error value.
- `File::open("hello.txt")` `Result<std::fs::File, std::io::Error>` qaytaradi; success bo'lsa [[file-handle|file handle]], failure bo'lsa [[io-error|io::Error]].
- `match` bilan `Ok(file)` va `Err(error)` variants explicit handle qilinadi.
- Hamma `Err` bir xil emas: [[error-kind|ErrorKind::NotFound]] bo'lsa file yaratish mumkin, boshqa I/O errorlarda panic yoki propagation tanlanishi mumkin.
- [[unwrap]] va [[expect]] `Result`/`Option`ni panic-on-error shortcut sifatida ochadi; `expect` message orqali assumptionni yaxshiroq hujjatlashtiradi.
- [[error-propagation|Error propagation]] errorni function ichida hal qilmasdan callerga `Err` sifatida qaytaradi.
- [[question-mark-operator|Question mark operator]] `?` manual `match` boilerplate'ini qisqartiradi: `Ok`ni ochadi, `Err`ni early return qiladi.
- `?` error type conversion uchun [[from-trait|From trait]]dan foydalanishi mumkin.
- `?` faqat compatible return type bo'lgan functionda ishlaydi; `()` qaytaradigan `main` ichida `Result` ustida `?` ishlatish [[e0277-trait-bound-not-satisfied|E0277]]ga olib keladi.
- [[main-function|main function]] `Result<(), E>` qaytarishi mumkin; `Ok(())` exit code 0, `Err` nonzero exit status beradi.
- `Box<dyn Error>` hozircha "any kind of error" deb o'qilishi mumkin; trait object tafsilotlari keyingi chapterlarda chuqurlashadi.
- 9.3 [[panic-vs-result|panic! vs Result]] qarorini yakunlaydi: fail bo'lishi mumkin bo'lgan function uchun `Result` yaxshi default, panic esa unrecoverable state yoki caller-side bug uchun.
- Examples, prototype code, va [[testing|tests]]da [[unwrap]] yoki [[expect]] ishlatish acceptable bo'lishi mumkin, chunki panic expected failure marker yoki test failure signalidir.
- Developer compilerdan ko'proq biladigan holatda `expect` mos: hardcoded valid IP parse qilinsa, message assumptionni hujjatlashtiradi.
- [[bad-state|Bad state]] - assumption, guarantee, [[function-contracts|contract]], yoki [[invariants|invariant]] buzilgan holat; continuing harmful yoki insecure bo'lsa panic to'g'ri bo'lishi mumkin.
- Expected failures - malformed parser input, HTTP rate limit, normal user input xatolari - `Result` orqali callerga qaytarilishi kerak.
- Rust type system ko'p validationni runtime check o'rniga compile time'ga olib chiqadi: `T` `Option<T>` emasligi value borligini bildiradi, `u32` negative bo'lmasligini bildiradi.
- [[custom-validation-types|Custom validation types]] patterni invariantni type ichida saqlaydi; [[guess-validation-type|Guess validation type]] 1..=100 range'ni private field va constructor orqali enforce qiladi.
- Public API panic qilishi mumkin bo'lgan shartlar documentationda yozilishi kerak.
- Chapter 9 complete: keyingi mavzu [[generics]].

## Concepts

- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[result|Result<T, E>]]
- [[panic|panic!]]
- [[stack-unwinding|stack unwinding]]
- [[abort-on-panic|abort on panic]]
- [[backtrace]]
- [[vector-indexing|vector indexing]]
- [[buffer-overread|buffer overread]]
- [[cargo-toml|Cargo.toml]]
- [[release-build|release build]]
- [[option|Option]]
- [[file-handle|file handle]]
- [[io-error|io::Error]]
- [[error-kind|ErrorKind]]
- [[unwrap]]
- [[expect]]
- [[error-propagation|error propagation]]
- [[question-mark-operator|question mark operator]]
- [[from-trait|From trait]]
- [[main-function|main function]]
- [[box-dyn-error|Box<dyn Error>]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
- [[panic-vs-result|panic! vs Result]]
- [[bad-state|bad state]]
- [[invariants]]
- [[function-contracts|function contracts]]
- [[custom-validation-types|custom validation types]]
- [[guess-validation-type|Guess validation type]]

## Examples

Explicit panic:

```rust
fn main() {
    panic!("crash and burn");
}
```

Release profile'da abort:

```toml
[profile.release]
panic = 'abort'
```

Out-of-bounds indexing:

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

Backtrace bilan tekshirish:

```shell
$ RUST_BACKTRACE=1 cargo run
```

Panic qilmaydigan alternative:

```rust
let v = vec![1, 2, 3];

match v.get(99) {
    Some(value) => println!("{value}"),
    None => println!("No value at that index"),
}
```

Open file and handle `Result`:

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

Handle missing file differently:

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

Propagate errors with `?`:

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

Use `?` in `main` by returning `Result`:

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

`expect` with a hardcoded invariant:

```rust
fn main() {
    use std::net::IpAddr;

    let home: IpAddr = "127.0.0.1"
        .parse()
        .expect("Hardcoded IP address should be valid");
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

## Exercises

- Quyidagi holatlarni recoverable yoki unrecoverable deb belgilang: missing file, invalid command-line flag, violated internal invariant, out-of-bounds index, network timeout.
- `v[99]` panic outputidan source line va panic message'ni ajrating.
- `RUST_BACKTRACE=1` outputida birinchi user-written file line'ni topish qoidasi bilan backtrace o'qing.
- `panic = 'abort'` settingi binary size, cleanup, va runtime behaviorga qanday ta'sir qilishini yozing.
- User input index bo'yicha vector access qiladigan kichik function yozing va `get` bilan `Option`ni handle qiling.
- `File::open` natijasini `match` bilan handle qiling: `NotFound` bo'lsa file yarating, boshqa error bo'lsa panic qiling.
- `unwrap` va `expect` panic message'larini solishtiring.
- Manual `return Err(e)` propagation functionini `?` bilan qayta yozing.
- `?` nima uchun compatible return type talab qilishini `main` example'i bilan tushuntiring.
- `Option` qaytaradigan functionda `?` orqali `None` early return bo'lishini yozing.
- Har bir holatda `panic!` yoki `Result` tanlang: malformed user input, broken invariant, HTTP rate limit, hardcoded valid IP parse, contract violation.
- `expect` message'ini assumptionni tushuntiradigan qilib qayta yozing.
- Guessing game range checkini `Guess` custom type bilan refactor qiling.
- Public API panic conditions uchun documentation comment yozing.

## Review Questions

- Rust nima uchun recoverable va unrecoverable errorsni alohida ko'radi?
- `panic!` va `Result<T, E>` orasidagi asosiy design farqi nima?
- `[]` indexing nega invalid indexda `None` qaytarmaydi?
- Backtrace o'qishda birinchi qaysi line'ga e'tibor berish kerak?
- `panic = 'abort'` qachon foydali bo'lishi mumkin?
- Rustning panic behaviori Cdagi buffer overread xavfini qanday kamaytiradi?
- `Result<T, E>`dagi `T` va `E` nimani bildiradi?
- `ErrorKind::NotFound` recovery decision uchun qanday ishlatiladi?
- `unwrap` va `expect` orasidagi amaliy farq nima?
- `?` operator errorni handle qiladimi yoki propagate qiladimi?
- `Result` ustidagi `?` va `Option` ustidagi `?` behaviorlari qanday o'xshash va qanday farqli?
- `main -> Result<(), Box<dyn Error>>` nimaga imkon beradi?
- Fail bo'lishi mumkin bo'lgan function uchun nima uchun `Result` yaxshi default?
- Examples/prototypes/testsda `unwrap` va `expect` qachon qabul qilinadi?
- Bad state va expected failure orasidagi farq nima?
- Contract violation nima uchun caller-side bug hisoblanadi?
- Custom validation type repeated runtime checksni qanday kamaytiradi?
- `Guess::new` panic qilishi documentationda qanday yozilishi kerak?

## Related Pages

- [[9-error-handling-the-rust-programming-language]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
- [[9-2-recoverable-errors-with-result-the-rust-programming-language]]
- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
- [[error-handling]]
- [[panic|panic!]]
- [[result|Result<T, E>]]
- [[vector-indexing|vector indexing]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[panic-vs-result|panic! vs Result]]
- [[custom-validation-types|custom validation types]]
