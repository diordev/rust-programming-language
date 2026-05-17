---
title: "while let"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, control-flow, pattern-matching]
source_count: 1
---

# while let

## Short Definition

`while let` — pattern mos kelgunicha loop davom etadigan shartli loop; `while` + pattern matching kombinatsiyasi.

## Why It Matters

Ba'zan iterator yoki channel'dan qiymatlar oqimi keladi va u `None` / `Err` qaytarguncha loop ishlash kerak. `while let` bu uchun eng qulay sintaksis.

## Mental Model

`while let PATTERN = EXPRESSION { ... }` ni "expression shu patternni hosil qilgunicha loop davom etsin" deb o'qing. Pattern mos kelmaguncha loop to'xtaydi.

[[if-let|if let]]ga o'xshash, lekin bitta tekshiruv emas — takrorlanuvchi shartli loop.

## Syntax and Examples

Asosiy sintaksis:
```rust
while let PATTERN = EXPRESSION {
    // body
}
```

Channel'dan xabarlar o'qish:
```rust
let (tx, rx) = std::sync::mpsc::channel();
std::thread::spawn(move || {
    for val in [1, 2, 3] {
        tx.send(val).unwrap();
    }
});

while let Ok(value) = rx.recv() {
    println!("{value}");
}
// tx yopilganda recv() Err qaytaradi → loop tugaydi
```

Stack'dan pop qilish:
```rust
let mut stack = vec![1, 2, 3];

while let Some(top) = stack.pop() {
    println!("{top}");  // 3, 2, 1
}
```

## Common Mistakes

- `while let` exhaustiveness tekshirmaydi — `if let` kabi faqat bir pattern uchun.
- Irrefutable pattern bilan `while let` ishlatish (masalan `while let x = get_value()`) compiler ogohlantiradi.
- Infinite loop xavfi: pattern doim mos kelsa loop hech qachon to'xtamaydi.

## Related Concepts

- [[if-let|if let]]
- [[loop]]
- [[pattern-matching|pattern matching]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[refutable-pattern|refutable pattern]]
- [[channels]]
- [[option|Option]]

## Sources

- [[wiki/sources/19-1-all-the-places-patterns-can-be-used]]
