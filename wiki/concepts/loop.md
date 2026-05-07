---
title: "loop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, control-flow]
source_count: 2
---

# loop

## Short Definition

`loop` Rustda infinite loop yaratadi. Loopdan chiqish uchun `break`, keyingi iterationga o'tish uchun `continue` ishlatiladi.

## Why It Matters

Interactive CLI programlarda user inputni qayta-qayta so'rash uchun kerak. Guessing gameda user to'g'ri topguncha loop davom etadi.

Chapter 3 `loop`dan tashqari `while` va `for` looplarni ham ko'rsatadi. Collection iteration uchun Rustda odatda `for` afzal.

## Mental Model

- `loop { ... }`: doimiy takrorlash.
- `break`: loopni tugatish.
- `continue`: hozirgi iterationni tashlab, keyingisiga o'tish.
- `break value`: loop expressiondan value qaytarish.
- Loop label: nested looplarda qaysi loopdan chiqishni aniqlash.

## Syntax and Examples

```rust
loop {
    let guess: u32 = match guess.trim().parse() {
        Ok(num) => num,
        Err(_) => continue,
    };

    if guess == secret_number {
        break;
    }
}
```

```rust
let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};
```

## Common Mistakes

- `break` condition yozmasdan accidental infinite loop yaratish.
- Invalid inputda `continue` o'rniga panic qilish.
- Collection bo'ylab yurishda manual `while` index bilan out-of-bounds panic yaratish.

## Related Concepts

- [[match]]
- [[result|Result]]
- [[control-flow|control flow]]
- [[while-loop|while loop]]
- [[for-loop|for loop]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[3-5-control-flow-the-rust-programming-language]]
