---
title: "Chapter 19.1: All the Places Patterns Can Be Used"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, destructuring, match, if-let, while-let]
source_count: 1
---

# Chapter 19.1: All the Places Patterns Can Be Used

## Learning Goal

Rustda patternlar ishlatilishi mumkin bo'lgan barcha 6 ta kontekstni bilish va ularning xususiyatlarini tushunish.

## Main Ideas

Pattern matching faqat `match`da emas, Rustning ko'p joyida ishlatiladi:

### 1. match Arms

Rasmiy sintaksis:
```rust
match VALUE {
    PATTERN => EXPRESSION,
}
```
Exhaustive bo'lishi shart. `_` yoki o'zgaruvchi nom bilan catch-all qilish mumkin.

### 2. let Statements

`let` aslida pattern ishlatadi:
```rust
let PATTERN = EXPRESSION;

let x = 5;           // x irrefutable pattern
let (x, y, z) = (1, 2, 3);  // tuple destructuring
```
Element soni mos kelmasa `E0308` compiler xatosi.

### 3. Conditional if let

`if let`, `else if`, `else if let`, `else` kombinatsiyasi:
```rust
if let Some(color) = favorite_color {
    // ...
} else if is_tuesday {
    // ...
} else if let Ok(age) = age {
    // age bu yerda shadowed
    if age > 30 { ... }
} else {
    // ...
}
```
**Muhim:** `if let` exhaustiveness tekshirmaydi. Unutilgan case bo'lsa compiler ogohlantirib bermaydi.

### 4. while let Conditional Loops

Pattern mos kelgunicha loop davom etadi:
```rust
while let Ok(value) = rx.recv() {
    println!("{value}");
}
// recv() Err qaytarsa to'xtaydi
```
Channel'lardan xabar o'qishda qulay.

### 5. for Loops

`for` dan keyin kelgan qism pattern:
```rust
for (index, value) in v.iter().enumerate() {
    println!("{value} is at index {index}");
}
```
`enumerate()` `(index, value)` tuple'lar qaytaradi; pattern ularni destructure qiladi.

### 6. Function Parameters

Funksiya parametrlari ham patternlar:
```rust
fn print_coordinates(&(x, y): &(i32, i32)) {
    println!("Current location: ({x}, {y})");
}
```
Closure parametrlari ham xuddi shunday ishlaydi.

## Concepts

- [[pattern-matching|pattern matching]]
- [[pattern-destructuring|pattern destructuring]]
- [[if-let|if let]]
- [[while-let|while let]]
- [[match]]
- [[exhaustive-matching|exhaustive matching]]
- [[irrefutable-pattern|irrefutable pattern]]

## Examples

```rust
// let destructuring
let (x, y, z) = (1, 2, 3);

// for destructuring
for (i, v) in vec.iter().enumerate() { ... }

// fn param destructuring
fn f(&(x, y): &(i32, i32)) { ... }

// while let channel
while let Ok(v) = rx.recv() { ... }
```

## Exercises

1. `let` bilan 4-elementli tuple destructuring yozing.
2. `for` + `enumerate()` bilan vector elementlarini indeks bilan chiqaring.
3. Channel'dan xabarlar qabul qiluvchi `while let` misolini yozing.
4. `if let` + `else if let` zanjiri bilan kamida 3 ta holat handle qiling.

## Review Questions

1. `let x = 5;`da nima pattern?
2. `if let` va `match` orasidagi asosiy farq nima?
3. `for (index, value) in v.iter().enumerate()`da nima pattern?
4. Funksiya parametrida destructuring qanday ishlaydi?

## Related Pages

- [[wiki/sources/19-1-all-the-places-patterns-can-be-used]]
- [[wiki/chapters/19-patterns-and-matching]]
- [[wiki/chapters/19-2-refutability-whether-a-pattern-might-fail-to-match]]
- [[wiki/chapters/6-enums-and-pattern-matching]]
