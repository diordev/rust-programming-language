---
title: "11.2. Controlling How Tests Are Run"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, cargo-test, parallel, filtering, ignore]
source_count: 1
---

# 11.2. Controlling How Tests Are Run

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: Chapter 11.2
- URL: https://doc.rust-lang.org/stable/book/ch11-02-running-tests.html
- Raw fayl: `raw/books/the_rust_programming_language/11.2. Controlling How Tests Are Run - The Rust Programming Language.md`

## Detailed Summary

### Argument ajratish: `--`

`cargo test` ikkita argument qatlamini qabul qiladi:

- `--` **oldingi** argumentlar → `cargo test` uchun (masalan, `--package`)
- `--` **keyingi** argumentlar → test binary uchun (masalan, `--test-threads`, `--show-output`)

```shell
cargo test --help          # cargo test uchun optsiyalar
cargo test -- --help       # test binary uchun optsiyalar
```

### Parallel va ketma-ket testlar

Default: testlar parallel thread'larda ishlaydi — tez tugaydi, tezroq feedback olasiz.

**Muammo:** Agar testlar shared state ishlatsa — masalan, bir xil fayl nomiga yozsa yoki bir xil environment variable'ga tayanса — parallel ishlatilganda bir-biriga xalaqit qilishi mumkin. Bunday holda test FAILED bo'ladi, kod xato bo'lmagani holda.

**Yechim 1:** Har bir test alohida fayl yoki resurs ishlatsin.  
**Yechim 2:** Thread sonini kamaytirish yoki birga ishlatish:

```shell
cargo test -- --test-threads=1
```

Bitta thread bilan testlar sekinroq, lekin o'zaro xalaqit qilmaydi.

### Output ushlab qolish (capture)

Default: **o'tgan testlarning `println!` outputi ushlanadi** — terminalda ko'rinmaydi. FAILED testlarning outputi esa ko'rsatiladi.

```rust
fn prints_and_returns_10(a: i32) -> i32 {
    println!("I got the value {a}");
    10
}

#[test]
fn this_test_will_pass() {
    let value = prints_and_returns_10(4);
    assert_eq!(value, 10);
    // "I got the value 4" - ko'rinmaydi (captured)
}

#[test]
fn this_test_will_fail() {
    let value = prints_and_returns_10(8);
    assert_eq!(value, 5);
    // "I got the value 8" - FAILED bo'lgani uchun ko'rinadi
}
```

O'tgan testlarning outputini ham ko'rish uchun:

```shell
cargo test -- --show-output
```

### Test filtrlash

**Bitta test nomi:**

```shell
cargo test one_hundred
```

Faqat nomi `one_hundred` bo'lgan test ishlaydi. Qolganlar `filtered out` bo'ladi.

**Substring bilan bir nechta:**

```shell
cargo test add
```

Nomi `add` substringini o'z ichiga olgan barcha testlar ishlaydi. Modul nomi ham qidiruv maydoniga kiradi: `tests::add_two_and_two` kabi to'liq nom bo'yicha.

```
running 2 tests
test tests::add_three_and_two ... ok
test tests::add_two_and_two ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out
```

**Cheklov:** Bir vaqtda bir nechta aniq nom bilan filtrlash imkoni yo'q — faqat bitta argument yoki substring.

### `#[ignore]` attribute

Vaqt talab qiladigan testlarni standart rundan chiqarish uchun:

```rust
#[test]
#[ignore]
fn expensive_test() {
    // bir soat ishlaydi
}
```

```shell
$ cargo test
# expensive_test ... ignored
# test result: ok. 1 passed; 0 failed; 1 ignored; ...
```

Faqat ignored testlarni ishlatish:

```shell
cargo test -- --ignored
```

Barcha testlarni, ignored ham:

```shell
cargo test -- --include-ignored
```

## Key Concepts

- [[testing]] — parallel, output capture, va ignore mexanizmlari
- [[test-filtering]] — test nomini yoki substringni argument sifatida berish

## Code Examples

- **Output capture** — Listing 11-10: `prints_and_returns_10`
- **Filtering** — Listing 11-11: `add_two_and_two`, `add_three_and_two`, `one_hundred`

## Exercises or Practice Ideas

- Bir xil faylga yozuvchi ikkita test yozing, parallel va `--test-threads=1` da ishlatib, farqni kuzating.
- `#[ignore]` bilan belgilangan test yozib, `-- --ignored` va `-- --include-ignored` ni sinang.
- `cargo test add` bilan filtrlashni uchta test ustida sinab ko'ring.

## Questions Raised

- `--test-threads` qiymati CPU core soniga qarab qanday tanlanadi?
- Async test framework'larda (`tokio::test`) parallel ishlatish qanday boshqariladi?

## Links To Update

- [[testing]] — parallel va ignore bo'limlarini qo'shish
- [[index]] — yangi source
- [[overview]] — Chapter 11 synthesis davomi
