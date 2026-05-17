---
title: "19.3. Pattern Syntax"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, pattern-syntax, destructuring, match-guard]
source_count: 1
---

# 19.3. Pattern Syntax

## Source

- URL: https://doc.rust-lang.org/stable/book/ch19-03-pattern-syntax.html
- Raw fayl: `raw/books/the_rust_programming_language/19.3. Pattern Syntax.md`

## Detailed Summary

Bu bo'lim Rustda pattern sintaksisining barcha variantlarini to'plagan reference bo'lim.

### Matching Literals

Pattern'ni to'g'ridan-to'g'ri literal qiymatlar bilan solishtirish:

```rust
let x = 1;
match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("anything"),
}
// "one" chiqaradi
```

Aniq bir qiymatda maxsus ish bajarish kerak bo'lganda foydali.

### Matching Named Variables

`match`, `if let`, `while let` yangi scope boshlaydi — pattern ichidagi nom tashqi o'zgaruvchini shadow qiladi.

```rust
let x = Some(5);
let y = 10;

match x {
    Some(50) => println!("Got 50"),
    Some(y) => println!("Matched, y = {y}"),  // yangi y, outer y = 10 emas!
    _ => println!("Default case, x = {x:?}"),
}
println!("at the end: x = {x:?}, y = {y}");  // y hali 10
```

`Some(y)` arm'idagi `y` — match scope'idagi yangi o'zgaruvchi, outer `y = 10` bilan aloqasi yo'q. Natija: `Matched, y = 5`.

Outer qiymat bilan solishtirish uchun [[match-guard|match guard]] kerak:
```rust
Some(n) if n == y => println!("Matched, n = {n}"),
```

### Matching Multiple Patterns (`|`)

`|` — [[or-pattern|or-operator]], bir arm'da bir nechta pattern:

```rust
match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("anything"),
}
```

Match guard bilan kombinatsiya: `if` butun `|` guruhiga tegishli:
```rust
4 | 5 | 6 if y => println!("yes"),  // (4|5|6) AND y, faqat 6 AND y emas
```

### Matching Ranges (`..=`)

Inklyuziv range pattern — faqat numeric (`i32`, `u8`, ...) va `char` uchun:

```rust
match x {
    1..=5 => println!("one through five"),
    _ => println!("something else"),
}
```

`char` uchun:
```rust
match x {
    'a'..='j' => println!("early ASCII letter"),
    'k'..='z' => println!("late ASCII letter"),
    _ => println!("something else"),
}
```

Compiler range bo'sh emasligini compile-time tekshiradi. `1 | 2 | 3 | 4 | 5` o'rniga `1..=5` yaxshiroq.

### Destructuring

#### Struct Destructuring

Field nomini o'zgaruvchiga boshqa nom bilan bog'lash:
```rust
let Point { x: a, y: b } = p;  // a = p.x, b = p.y
```

Shorthand (nom bir xil):
```rust
let Point { x, y } = p;        // x = p.x, y = p.y
```

Literallar bilan aralash — ba'zi fieldlar test qilinadi, boshqalari bind:
```rust
match p {
    Point { x, y: 0 } => println!("On x axis at {x}"),   // y == 0 bo'lsa
    Point { x: 0, y } => println!("On y axis at {y}"),   // x == 0 bo'lsa
    Point { x, y } => println!("Neither ({x}, {y})"),
}
```

#### Enum Destructuring

Enum variant shakliga qarab mos pattern:

```rust
match msg {
    Message::Quit => { ... }                             // data yo'q
    Message::Move { x, y } => { ... }                   // struct-like
    Message::Write(text) => { ... }                     // tuple-like (1 element)
    Message::ChangeColor(r, g, b) => { ... }            // tuple-like (3 element)
}
```

#### Nested Enum/Struct

Bir nechta darajada nested destructuring:
```rust
match msg {
    Message::ChangeColor(Color::Rgb(r, g, b)) => { ... },
    Message::ChangeColor(Color::Hsv(h, s, v)) => { ... },
    _ => (),
}
```

#### Aralash (Struct + Tuple)

```rust
let ((feet, inches), Point { x, y }) = ((3, 10), Point { x: 3, y: -10 });
```

### Ignoring Values

#### `_` — Butun qiymatni ignore

Funksiya param sifatida (trait implementation'da kerak bo'lmagan param):
```rust
fn foo(_: i32, y: i32) { println!("{y}"); }
```

Nested — qiymatning faqat bir qismini ignore:
```rust
match (setting_value, new_setting_value) {
    (Some(_), Some(_)) => println!("Can't overwrite"),
    _ => setting_value = new_setting_value,
}
```

Tuple'ning ba'zi elementlarini ignore:
```rust
let numbers = (2, 4, 8, 16, 32);
match numbers {
    (first, _, third, _, fifth) => println!("{first}, {third}, {fifth}"),
}
```

#### `_name` — Bind qiladi, warning bermaydi

`_x` va `_` o'rtasidagi farq muhim:
- `_x` — qiymatni bind qiladi (move ham qiladi!)
- `_` — bind qilmaydi

```rust
let s = Some(String::from("Hello!"));
if let Some(_s) = s {     // _s ga move bo'ladi → s dan keyin foydalanib bo'lmaydi
    println!("found");
}
// println!("{s:?}");  // XATO: s moved
```

```rust
let s = Some(String::from("Hello!"));
if let Some(_) = s {      // move bo'lmaydi
    println!("found");
}
println!("{s:?}");        // OK: Some("Hello!")
```

#### `..` — Qolganlarini ignore

Struct'da kerak bo'lmagan fieldlarni o'tkazish:
```rust
match origin {
    Point { x, .. } => println!("x is {x}"),
}
```

Tuple'da birinchi va oxirgi:
```rust
match numbers {
    (first, .., last) => println!("{first}, {last}"),
}
```

`..` noaniq bo'lmasligi kerak — `(.., second, ..)` compile xatosi.

### Match Guards

Pattern'ga qo'shimcha `if` sharti — faqat `match`da ishlaydi:

```rust
match num {
    Some(x) if x % 2 == 0 => println!("even: {x}"),
    Some(x) => println!("odd: {x}"),
    None => (),
}
```

Outer o'zgaruvchi bilan solishtirish (shadowing muammosini hal qiladi):
```rust
let y = 10;
match x {
    Some(n) if n == y => println!("Matched, n = {n}"),  // outer y ishlatiladi
    _ => println!("no match"),
}
```

`|` bilan kombinatsiya — `if` butun guruhga tegishli:
```rust
4 | 5 | 6 if y => ...   // ya'ni: (4 | 5 | 6) if y
```

Match guard'lar exhaustiveness tekshiruvini qiyinlashtiradi (compiler to'liq tahlil qila olmaydi).

### `@` Bindings

Qiymatni ham test qilish, ham o'zgaruvchiga bind qilish:

```rust
match msg {
    Message::Hello { id: id @ 3..=7 } => println!("id in range: {id}"),
    Message::Hello { id: 10..=12 } => println!("id in 10-12"),  // id mavjud emas!
    Message::Hello { id } => println!("other id: {id}"),
}
```

Ikkinchi arm'da `id` o'zgaruvchi yo'q (faqat range test). `@` birinchi arm'da range test + binding ikkalasini bajaradi.

## Key Concepts

- [[pattern-matching|pattern matching]]
- [[or-pattern|or-pattern (`|`)]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[pattern-destructuring|pattern destructuring]]
- [[catch-all-patterns|catch-all patterns]]
- [[shadowing]]

## Code Examples

```rust
// Range pattern
1..=5 => ...
'a'..='j' => ...

// Struct destructuring
let Point { x, y } = p;
let Point { x: a, y: b } = p;

// Or-pattern
1 | 2 => println!("one or two"),

// Match guard
Some(x) if x % 2 == 0 => ...

// @ binding
id @ 3..=7 => println!("{id}")

// .. ignore
Point { x, .. } => ...
(first, .., last) => ...

// _name vs _
if let Some(_s) = s { ... }   // moves s
if let Some(_) = s { ... }    // doesn't move s
```

## Exercises or Practice Ideas

1. `_name` vs `_` farqini `String` bilan sinab ko'ring.
2. `..=` range pattern bilan ASCII harflarni kategoriyalang.
3. Match guard bilan juft/toq sonlarni ajrating.
4. Nested `Color::Rgb` / `Color::Hsv` enum'ni destructure qiling.
5. `@` binding bilan ID validatsiyasi yozing.
6. `..` bilan 5-elementli tuple'dan faqat birinchi va oxirgi ni oling.

## Questions Raised

- Match guard'lar exhaustiveness'ga qanday ta'sir qiladi?
- `@` binding range'siz ham ishlaydi?
- `..` struct'da bitta marta ishlatish kifoya qiladimi?

## Links To Update

- [[wiki/chapters/19-3-pattern-syntax]]
- [[pattern-matching]] — 19.3 source qo'shish
- [[pattern-destructuring]] — struct/enum destructuring qo'shish
- [[catch-all-patterns]] — `..` va `_name` qo'shish
- [[match-guard]] — yangi concept
- [[at-binding]] — yangi concept
- [[or-pattern]] — yangi concept
