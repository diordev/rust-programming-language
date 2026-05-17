---
title: "Or-Pattern (`|`)"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, match]
source_count: 1
---

# Or-Pattern (`|`)

## Short Definition

`|` — pattern "yoki" operatori; bitta match arm'ida bir nechta pattern'ni birlashtirib, ulardan biri mos kelsa arm ishlaydigan sintaksis.

## Why It Matters

Bir xil mantiqni bajaradigan bir nechta qiymat uchun alohida arm yozish o'rniga `|` bilan ixchamlashtirish mumkin. `1..=5` range o'rniga `1 | 2 | 3 | 4 | 5` yozishdan saqlaydigan sintaktik qulaylik ham mavjud, lekin range ko'pincha afzalroq.

## Mental Model

`PATTERN1 | PATTERN2 | PATTERN3 => EXPR` — "qiymat ulardan biriga mos kelsa". Har bir pattern bir xil arm ishga tushiradi.

## Syntax and Examples

Asosiy:
```rust
match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("other"),
}
```

`if let` yoki `while let` da ham:
```rust
if let 'a' | 'e' | 'i' | 'o' | 'u' = ch {
    println!("vowel");
}
```

Match guard bilan kombinatsiya — `if` butun `|` guruhiga tegishli:
```rust
match x {
    4 | 5 | 6 if y => println!("yes"),  // (4 | 5 | 6) if y
    _ => println!("no"),
}
```

`4 | 5 | (6 if y)` deb o'qilmaydi — `if` oxirgi elementga emas, butun guruhga bog'liq.

## Common Mistakes

- `|` bilan [[match-guard|match guard]] aralashtirganda `if` qayerga tegishli ekanini unutish.
- `let`da or-pattern ishlatish (`let 1 | 2 = x;`) — mumkin emas (irrefutable bo'lishi kerak).

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[match-guard|match guard]]
- [[catch-all-patterns|catch-all patterns]]

## Sources

- [[wiki/sources/19-3-pattern-syntax]]
