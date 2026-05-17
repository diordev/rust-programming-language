---
title: "19.1. All the Places Patterns Can Be Used"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, pattern-matching, destructuring]
source_count: 1
---

# 19.1. All the Places Patterns Can Be Used

## Source

- URL: https://doc.rust-lang.org/stable/book/ch19-01-all-the-places-for-patterns.html
- Raw fayl: `raw/books/the_rust_programming_language/19.1. All the Places Patterns Can Be Used.md`

## Detailed Summary

Ushbu bo'lim Rustda patternlar ishlatilishi mumkin bo'lgan barcha joylarni ko'rib chiqadi. Ko'pincha biz ularni sezmasdan ishlatib kelganmiz.

### 1. match Arms

`match` expressionining har bir arm'i pattern'dan iborat:

```rust
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
```

`match` [[exhaustive-matching|exhaustive]] bo'lishi shart — barcha holatlar yopilib ketishi kerak. Oxirgi arm uchun `_` wildcard yoki o'zgaruvchi nomi qolgan casesni yopadi.

```rust
match x {
    None => None,
    Some(i) => Some(i + 1),
}
```

### 2. let Statements

`let` iborasi aslida pattern ishlatadi:

```rust
let PATTERN = EXPRESSION;
```

`let x = 5;`da `x` — oddiy irrefutable pattern. Lekin `let` tuple destructuring ham qila oladi:

```rust
let (x, y, z) = (1, 2, 3);  // x=1, y=2, z=3
```

Tuple element soni mos kelmasa compiler `E0308` xatosini beradi:

```rust
let (x, y) = (1, 2, 3);  // Xato! 3 element, 2 o'zgaruvchi
```

### 3. Conditional if let Expressions

`if let` bitta pattern uchun concise matching beradi. `else if`, `else if let`, `else` bilan kombinatsiya qilish mumkin:

```rust
if let Some(color) = favorite_color {
    println!("Using {color}");
} else if is_tuesday {
    println!("Tuesday is green day!");
} else if let Ok(age) = age {
    if age > 30 {
        println!("Using purple");
    } else {
        println!("Using orange");
    }
} else {
    println!("Using blue");
}
```

Muhim: `if let Ok(age) = age` yangi `age` o'zgaruvchi yaratadi, eski `age`ni shadow qiladi. Shuning uchun `if let Ok(age) = age && age > 30` yozib bo'lmaydi — yangi `age` faqat `{` dan keyin mavjud.

`if let` [[exhaustive-matching|exhaustiveness]] tekshirmaydi (match'dan farqli). Forgotten case bo'lsa compiler ogohlantirir emasdi.

### 4. while let Conditional Loops

`while let` — pattern mos kelgunicha loop davom etadi:

```rust
let (tx, rx) = std::sync::mpsc::channel();
std::thread::spawn(move || {
    for val in [1, 2, 3] {
        tx.send(val).unwrap();
    }
});

while let Ok(value) = rx.recv() {
    println!("{value}");  // 1, 2, 3 chiqaradi
}
// rx.recv() Err qaytarsa loop to'xtaydi
```

Channel'dan xabarlar kelguncha ishlaydi; sender yopilsa `Err` qaytadi va loop tugaydi.

### 5. for Loops

`for` loopda `for` kalit so'zidan keyin kelgan qism pattern:

```rust
let v = vec!['a', 'b', 'c'];

for (index, value) in v.iter().enumerate() {
    println!("{value} is at index {index}");
}
// a is at index 0
// b is at index 1
// c is at index 2
```

`enumerate()` `(0, 'a')`, `(1, 'b')`, ... kabi tuple'lar qaytaradi. `(index, value)` pattern ularni destructure qiladi.

### 6. Function Parameters

Funksiya parametrlari ham patternlar:

```rust
fn foo(x: i32) { ... }  // x — pattern
```

Tuple destructuring parameter sifatida:

```rust
fn print_coordinates(&(x, y): &(i32, i32)) {
    println!("Current location: ({x}, {y})");
}

fn main() {
    let point = (3, 5);
    print_coordinates(&point);  // Current location: (3, 5)
}
```

Closures ham funksiya parameterlariga o'xshab pattern qabul qila oladi.

## Key Concepts

- [[pattern-matching|pattern matching]]
- [[match]]
- [[if-let|if let]]
- [[while-let|while let]]
- [[pattern-destructuring|pattern destructuring]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[channels]]

## Code Examples

```rust
// let destructuring
let (x, y, z) = (1, 2, 3);

// for loop destructuring
for (index, value) in v.iter().enumerate() { ... }

// fn param destructuring
fn print_coordinates(&(x, y): &(i32, i32)) { ... }

// while let channel
while let Ok(value) = rx.recv() { println!("{value}"); }
```

## Exercises or Practice Ideas

- `let` bilan 5-elementli tuple destructuring yozing.
- `for` loopda `enumerate()` ishlatib indexli chiqarish.
- `while let` bilan channel'dan xabarlar qabul qiluvchi dastur yozing.
- `if let` + `else if let` zanjiri bilan murakkab shartli mantiq yozing.

## Questions Raised

- `if let`da exhaustiveness yo'qligi qachon xatoga olib kelishi mumkin?
- `while let` vs `for` loop qaysi holatlarda afzalroq?

## Links To Update

- [[wiki/chapters/19-1-all-the-places-patterns-can-be-used]]
- [[pattern-matching]] — source qo'shish
- [[if-let]] — 19.1 dan shadowing misoli qo'shish
- [[while-let]] — yangi concept
- [[pattern-destructuring]] — yangi concept
