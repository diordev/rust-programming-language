---
title: "@ Binding"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, patterns, binding]
source_count: 2
---

# @ Binding

## Short Definition

`@` operator — pattern ichida qiymatni test qilish bilan bir vaqtda uni o'zgaruvchiga bog'lash imkonini beradi.

## Why It Matters

Ba'zan qiymat ma'lum bir range yoki shaklga mos kelishini tekshirish va bir vaqtda uni ishlatish kerak bo'ladi. Faqat range pattern (`10..=12`) ishlatilsa — qiymat bind qilinmaydi va arm ichida ishlatib bo'lmaydi. `@` bu ikki ishni bitta yozuvda bajaradi.

## Mental Model

`variable @ PATTERN` — "shu pattern'ga mos kelsa, uni `variable` deb nom ber". Test ham o'tadi, nom ham beriladi.

## Syntax and Examples

```rust
enum Message {
    Hello { id: i32 },
}

let msg = Message::Hello { id: 5 };

match msg {
    Message::Hello { id: id @ 3..=7 } => {
        println!("Found id in range 3-7: {id}")  // id ishlatilishi mumkin
    }
    Message::Hello { id: 10..=12 } => {
        println!("Found id in 10-12")  // id yo'q! qiymatni bilmaymiz
    }
    Message::Hello { id } => println!("Other id: {id}"),
}
// "Found id in range 3-7: 5"
```

Range'siz `@` — o'zgaruvchi nomi bilan pattern:
```rust
let x = 5;
match x {
    n @ 1..=10 => println!("1-10 orasida: {n}"),
    _ => println!("boshqa"),
}
```

## Common Mistakes

- `@` o'rniga faqat range yozish (masalan `10..=12`) — arm ichida qiymatni ishlatib bo'lmaydi.
- `@` sintaksisi: `id @ 3..=7` (field renaming bilan: `{ id: id @ 3..=7 }`).

## Related Concepts

- [[pattern-matching|pattern matching]]
- [[match]]
- [[match-guard|match guard]]
- [[irrefutable-pattern|irrefutable pattern]]

## Sources

- [[wiki/sources/19-3-pattern-syntax]]
- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
