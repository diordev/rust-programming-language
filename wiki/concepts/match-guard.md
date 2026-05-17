---
title: "Match Guard"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, match, pattern-matching]
source_count: 2
---

# Match Guard

## Short Definition

`match` arm'iga qo'shimcha `if` sharti — pattern mos kelgandan keyin shu shart ham bajarilishi kerak bo'lsa arm tanlanadi.

## Why It Matters

Ba'zan pattern ifodalay olmaydigan murakkab shartlar kerak bo'ladi. Masalan, `Some(x)` pattern'i `x` ning juft-toqligini tekshira olmaydi. Match guard shu imkoniyatni beradi. Shuningdek, match scope'dagi shadowing muammosini hal qilishda ham foydali — outer o'zgaruvchi bilan solishtirish uchun.

## Mental Model

`PATTERN if CONDITION => EXPRESSION`. Avval pattern mos keladi, keyin `if` bajariladi. Ikkisi ham true bo'lsa arm ishlaydi. Guard pattern ichida bind qilingan nomlarga ham, outer scope'dagi nomlarga ham murojaat qila oladi. Match guard faqat `match` expressionida mavjud — `if let` va `while let` qo'llab-quvvatlamaydi.

## Syntax and Examples

Juft sonni tanlash:
```rust
let num = Some(4);
match num {
    Some(x) if x % 2 == 0 => println!("even: {x}"),
    Some(x) => println!("odd: {x}"),
    None => (),
}
// "even: 4"
```

Outer o'zgaruvchi bilan solishtirish (pattern-shadowing muammosini hal qilish):
```rust
let x = Some(5);
let y = 10;

match x {
    Some(50) => println!("Got 50"),
    Some(n) if n == y => println!("Matched, n = {n}"), // outer y ishlatiladi
    _ => println!("Default, x = {x:?}"),
}
// "Default, x = Some(5)"  — chunki 5 != 10
```

`|` bilan kombinatsiya — `if` butun `|` guruhiga tegishli:
```rust
let x = 4;
let y = false;

match x {
    4 | 5 | 6 if y => println!("yes"),  // (4|5|6) AND y
    _ => println!("no"),
}
// "no" — y = false
```

Bu `4 | 5 | (6 if y)` emas, balki `(4 | 5 | 6) if y` deb o'qiladi.

## Common Mistakes

- Match guard faqat `match`da ishlaydi; `if let` yoki `while let` da ishlatish mumkin emas.
- `4 | 5 | 6 if y`da `if y` faqat `6`ga tegishli deb o'ylash — aslida butun guruhga.
- Match guard compiler exhaustiveness tekshiruvini qiyinlashtiradi; ba'zi holatlarda compiler barcha casesni qamrab olganligini tekshira olmaydi.

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[shadowing]]
- [[or-pattern|or-pattern (`|`)]]
- [[exhaustive-matching|exhaustive matching]]

## Sources

- [[wiki/sources/19-3-pattern-syntax]]
- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
