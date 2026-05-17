---
title: "E0277 trait bound not satisfied"
type: error
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, compiler-error, traits, debug]
source_count: 7
---

# E0277 trait bound not satisfied

## Symptom

Type kerakli traitni implement qilmaganida `error[E0277]` chiqishi mumkin. Tushib yo'liqishi mumkin bo'lgan ikkita keng tarqalgan kontekst:

1. Chapter 5.2 — custom `Rectangle`ni `println!("{rect1}")` yoki `println!("{rect1:?}")` bilan chiqarishga urinish.
2. Chapter 6.1 — `Option<i8>`'ni `i8`'ga to'g'ridan-to'g'ri qo'shishga urinish.
3. Chapter 8.2 - `String` yoki `str`ni integer index bilan access qilishga urinish.
4. Chapter 9.2 - `?` operatorni `()` qaytaradigan functionda `Result` ustida ishlatishga urinish.
5. Chapter 10.2 - trait bound talab qilgan functionga traitni implement qilmagan type berishga urinish.
6. Chapter 15.2 - integer value bilan reference'ni to'g'ridan-to'g'ri compare qilishga urinish.

## Cause

**Format placeholderlari uchun:** `{}` [[display-formatting|Display formatting]] talab qiladi; Rust custom [[structs|struct]] uchun `Display`'ni avtomatik implement qilmaydi. `{:?}` esa [[debug-formatting|Debug formatting]] talab qiladi; struct `Debug`'ni explicit opt in qilmasa, compiler implementation yo'qligini aytadi.

**`Option<T>` + `T` uchun:** `Option<T>` va `T` boshqa-boshqa typelar. `i8 + Option<i8>` uchun `Add<Option<i8>>` trait `i8` uchun implement qilinmagan. Bu Rust dizayni: [[option|Option<T>]] ichidagi value'ni ishlatishdan oldin `T`'ga aylantirib olish kerak.

**String integer indexing uchun:** `str` integer index bilan indexed bo'lmaydi, chunki [[utf-8|UTF-8]]da byte index, Unicode scalar value, va [[grapheme-clusters|grapheme cluster]] bir xil emas. Rust `s[0]` nimani qaytarishini noaniq qoldirmaydi.

**`?` operator uchun:** `?` errorni current functiondan early return qiladi. Shuning uchun function return type'i `Result`, `Option`, yoki compatible residual type bo'lishi kerak. `fn main()` default `()` qaytaradi, shuning uchun `File::open("hello.txt")?` bu signature ichida ishlamaydi.

**Trait bound uchun:** `fn notify<T: Summary>(item: &T)` faqat `Summary` implement qilgan typelarni qabul qiladi. `String` yoki `i32` `Summary` implement qilmagan bo'lsa, compiler kerakli trait bound bajarilmaganini bildiradi.

**Reference compare uchun:** `assert_eq!(5, y)` da `y: &i32` bo'lsa, `i32` bilan `&i32` solishtirilmoqda. Avval [[dereference-operator|dereference operator]] bilan `*y` orqali inner value'ga yetish kerak.

## Fix Pattern

Developer debugging uchun `Debug` derive qiling va `{:?}` yoki `{:#?}` ishlating:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

println!("rect1 is {rect1:?}");
```

User-facing output uchun keyinchalik `Display`ni custom implement qilish kerak bo'ladi.

Trait bound xatosida yoki type uchun kerakli traitni implement qiling, yoki functionga boundni qanoatlantiradigan type bering.

Reference bilan value solishtirilayotgan bo'lsa, reference'ni dereference qiling:

```rust
assert_eq!(5, *y);
```

## Minimal Example

### Custom struct + format placeholder

Failing:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    println!("rect1 is {rect1:?}");
}
```

Fixed:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    println!("rect1 is {rect1:?}");
}
```

### Option<T> + T

Failing:

```rust
fn main() {
    let x: i8 = 5;
    let y: Option<i8> = Some(5);
    let sum = x + y; // E0277: no implementation for `i8 + Option<i8>`
}
```

Fixed (`Option`ni avval `T`ga aylantiring):

```rust
fn main() {
    let x: i8 = 5;
    let y: Option<i8> = Some(5);
    let sum = x + y.unwrap_or(0);
}
```

Yoki `match` bilan har ikki branchni handle qiling:

```rust
fn main() {
    let x: i8 = 5;
    let y: Option<i8> = Some(5);
    let sum = match y {
        Some(n) => x + n,
        None => x,
    };
}
```

### String indexing

Failing:

```rust
fn main() {
    let s1 = String::from("hi");
    let h = s1[0];
}
```

Explicit alternatives:

```rust
for c in "Зд".chars() {
    println!("{c}");
}

for b in "Зд".bytes() {
    println!("{b}");
}
```

### `?` in `main` returning `()`

Failing:

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")?;
}
```

Fixed:

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

### Trait bound not satisfied

Failing idea:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}

fn main() {
    let text = String::from("hello");
    notify(&text);
}
```

Fixed idea:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct SocialPost {
    username: String,
    content: String,
}

impl Summary for SocialPost {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

### Comparing value with reference

Failing:

```rust
fn main() {
    let x = 5;
    let y = &x;

    assert_eq!(5, y);
}
```

Fixed:

```rust
fn main() {
    let x = 5;
    let y = &x;

    assert_eq!(5, *y);
}
```

## Thread kontekstida E0277 — `Send` emas

`thread::spawn` `Send` bound talab qiladi. `Rc<T>` `Send` implement qilmaydi:

```rust
use std::rc::Rc;
use std::sync::Mutex;
use std::thread;

let counter = Rc::new(Mutex::new(0));
let counter2 = Rc::clone(&counter);
thread::spawn(move || {
    *counter2.lock().unwrap() += 1;
    // XATO: `Rc<Mutex<i32>>` cannot be sent between threads safely
    // the trait `Send` is not implemented for `Rc<Mutex<i32>>`
});
```

**Yechim:** `Rc<T>` o'rniga `Arc<T>` ishlatish — `Arc<T>: Send + Sync`.

## Related Concepts

- [[debug-trait|Debug trait]]
- [[derive-attribute|derive attribute]]
- [[debug-formatting|Debug formatting]]
- [[display-formatting|Display formatting]]
- [[traits]]
- [[structs]]
- [[option|Option]]
- [[enums]]
- [[reference]]
- [[dereference-operator]]
- [[string-indexing|string indexing]]
- [[utf-8|UTF-8]]
- [[question-mark-operator|question mark operator]]
- [[main-function|main function]]
- [[result|Result<T, E>]]
- [[box-dyn-error|Box<dyn Error>]]
- [[trait-bounds|trait bounds]]
- [[impl-trait|impl Trait]]
- [[trait-implementations|trait implementations]]
- [[send-trait|Send trait]]
- [[arc-t|Arc<T>]]
- [[rc-t|Rc<T>]]
- [[threads]]

## Sources

- [[5-2-an-example-program-using-structs]]
- [[6-1-defining-an-enum]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[9-2-recoverable-errors-with-result]]
- [[10-2-defining-shared-behavior-with-traits]]
- [[15-2-treating-smart-pointers-like-regular-references]]
- [[16-3-shared-state-concurrency]]
