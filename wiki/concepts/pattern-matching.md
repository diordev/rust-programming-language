---
title: "Pattern Matching"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, control-flow]
source_count: 4
---

# Pattern Matching

## Short Definition

`pattern matching` value shaklini tekshirib, mos branchni tanlash va kerak bo'lsa value ichidagi data'ni binding orqali ajratib olish usuli.

## Why It Matters

Rustda pattern matching [[result|Result]], [[option|Option]], va custom [[enums|enum]] variantlarini type-safe handle qilish uchun asosiy vosita. Compiler [[exhaustive-matching|exhaustive matching]] orqali unutilgan casesni ushlaydi.

## Mental Model

Pattern "value qanday ko'rinishda?" degan savolga javob beradi. `Some(i)` patterni "bu value `Some` variantidami?" deb tekshiradi va ichidagi qiymatni `i`ga bind qiladi. `Coin::Quarter(state)` patterni enum variantini ham, payloadni ham tekshiradi.

Patterns `match`, [[if-let|if let]], va [[let-else|let...else]]da ishlatiladi. `match` barcha casesni yopishni talab qiladi; `if let` va `let...else` bitta pattern atrofida concise flow beradi.

## Syntax and Examples

```rust
match result {
    Ok(value) => value,
    Err(_) => return,
}
```

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

```rust
if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

```rust
let Some(name) = maybe_name else {
    return;
};
```

## Common Mistakes

- `match` arm'lari bir xil type qaytarishi kerakligini unutish.
- Exhaustive cases kerakligini hisobga olmaslik.
- `_` catch-all pattern bilan yangi variantlar haqida compiler eslatmasini yo'qotish.
- Pattern binding scope'ini unutish: `if let` bindingi block ichida, `let...else` bindingi outer scope'da.

## Related Concepts

- [[match]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[option|Option]]
- [[enums]]
- [[result|Result]]
- [[ordering|Ordering]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]

## Sources

- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[0-2-introduction-the-rust-programming-language]]
- [[6-2-the-match-control-flow-construct-the-rust-programming-language]]
- [[6-3-concise-control-flow-with-if-let-and-let-else-the-rust-programming-language]]
