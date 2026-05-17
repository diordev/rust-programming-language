---
title: "Chapter 19.3: Pattern Syntax"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, pattern-syntax, destructuring, match-guard]
source_count: 1
---

# Chapter 19.3: Pattern Syntax

## Learning Goal

Rustda ishlatilishi mumkin bo'lgan barcha pattern sintaksis variantlarini bilish: literals, named variables, `|`, `..=`, destructuring, `_`, `..`, match guards, `@` bindings.

## Main Ideas

### 1. Literals va Named Variables

Literal pattern — aniq qiymat bilan mos kelish:
```rust
match x {
    1 => println!("one"),
    2 => println!("two"),
    _ => println!("other"),
}
```

Named variable — match scope'da yangi o'zgaruvchi (outer'ni shadow qiladi):
```rust
let y = 10;
match x {
    Some(y) => println!("{y}"),  // bu y — outer y emas, match ichidagi yangi y!
    _ => ()
}
// outer y hali 10
```

Outer qiymatni solishtirish uchun → [[match-guard|match guard]] kerak.

### 2. `|` Or-Pattern va `..=` Range

Bir arm'da bir nechta pattern:
```rust
1 | 2 => println!("one or two"),
```

Inklyuziv range (faqat numeric va `char`):
```rust
1..=5 => println!("one through five"),
'a'..='j' => println!("early letter"),
```

### 3. Destructuring

**Struct:**
```rust
let Point { x, y } = p;              // shorthand
let Point { x: a, y: b } = p;        // rename
Point { x, y: 0 } => println!("{x}") // literal field test
```

**Enum (variant shakliga qarab):**
```rust
Message::Quit => { ... }             // data yo'q
Message::Move { x, y } => { ... }    // struct-like
Message::Write(text) => { ... }      // tuple-like
Message::ChangeColor(r, g, b) => { ... }
```

**Nested enum/struct:**
```rust
Message::ChangeColor(Color::Rgb(r, g, b)) => { ... }
```

**Struct + tuple aralash:**
```rust
let ((feet, inches), Point { x, y }) = ((3, 10), Point { x: 3, y: -10 });
```

### 4. Qiymatlarni Ignore Qilish

| Sintaksis | Xatti-harakat |
|---|---|
| `_` | Bind qilmaydi, ignore qiladi |
| `_name` | Bind qiladi (move!), warning bermaydi |
| `..` | Qolgan barcha qismlarni ignore |
| Nested `_` | Qiymatning faqat bir qismini ignore |

**Muhim farq: `_` vs `_name`:**
```rust
if let Some(_s) = s { ... }  // s moved — keyin ishlatib bo'lmaydi!
if let Some(_) = s { ... }   // s moved emas — OK
```

**`..` struct va tuple'da:**
```rust
Point { x, .. } => ...           // y, z ignore
(first, .., last) => ...         // o'rtadagilar ignore
// (.., second, ..) — XATO: noaniq
```

### 5. Match Guards

Pattern'dan keyin `if` sharti — faqat `match`da:
```rust
Some(x) if x % 2 == 0 => println!("even"),
```

Outer o'zgaruvchi bilan solishtirish:
```rust
Some(n) if n == y => println!("equal to outer y"),
```

`|` bilan: `if` butun guruhga tegishli:
```rust
4 | 5 | 6 if cond => ...   // (4|5|6) AND cond
```

### 6. `@` Bindings

Test va bind bitta yozuvda:
```rust
id @ 3..=7 => println!("id in range: {id}"),
// Faqat range: 10..=12 => ...  // id mavjud emas
```

## Concepts

- [[pattern-matching|pattern matching]]
- [[or-pattern|or-pattern (`|`)]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[pattern-destructuring|pattern destructuring]]
- [[catch-all-patterns|catch-all patterns]]
- [[shadowing]]
- [[irrefutable-pattern|irrefutable pattern]]

## Examples

```rust
// Match guard — outer var bilan solishtirish
let y = 10;
match x {
    Some(n) if n == y => println!("Matched: {n}"),
    _ => println!("No match"),
}

// @ binding
match msg {
    Message::Hello { id: id @ 3..=7 } => println!("{id}"),
    Message::Hello { id } => println!("{id}"),
}

// _name vs _
let s = Some(String::from("hi"));
if let Some(_) = s { ... }  // s saqlanib qoladi
```

## Exercises

1. `_name` bilan `_` farqini `Option<String>` ustida sinab ko'ring.
2. Match guard bilan 1..=100 orasida juft sonni toping.
3. Nested `Color::Rgb` / `Color::Hsv` enum'ni destructure qiling.
4. `@` binding bilan ID 1..=10 bo'lsa "valid", boshqa bo'lsa "invalid" chiqaring.
5. `..` bilan struct'ning faqat bitta fieldini oling.
6. `|` + match guard kombinatsiyasini (`4|5|6 if y`) sinab ko'ring.

## Review Questions

1. `match` ichida named variable tashqi o'zgaruvchini shadow qiladimi? Misol.
2. `_s` va `_` o'rtasida ownership bo'yicha qanday farq bor?
3. `..` tuple'da qanday ishlatiladi? Noaniq holatda nima bo'ladi?
4. Match guard faqat `match`da ishlaydi — nima uchun?
5. `@` binding nimaga kerak? Faqat range pattern bilan ishladimi?
6. `4 | 5 | 6 if y` qanday evalyatsiya qilinadi?

## Related Pages

- [[wiki/sources/19-3-pattern-syntax]]
- [[wiki/chapters/19-patterns-and-matching]]
- [[wiki/chapters/19-1-all-the-places-patterns-can-be-used]]
- [[wiki/chapters/19-2-refutability-whether-a-pattern-might-fail-to-match]]
