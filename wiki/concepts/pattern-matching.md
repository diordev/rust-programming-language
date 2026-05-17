---
title: "Pattern Matching"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, control-flow]
source_count: 10
---

# Pattern Matching

## Short Definition

`pattern matching` value shaklini tekshirib, mos branchni tanlash va kerak bo'lsa value ichidagi data'ni binding orqali ajratib olish usuli.

## Why It Matters

Rustda pattern matching [[result|Result]], [[option|Option]], va custom [[enums|enum]] variantlarini type-safe handle qilish uchun asosiy vosita. Compiler [[exhaustive-matching|exhaustive matching]] orqali unutilgan casesni ushlaydi.

## Mental Model

Pattern "value qanday ko'rinishda?" degan savolga javob beradi. `Some(i)` patterni "bu value `Some` variantidami?" deb tekshiradi va ichidagi qiymatni `i`ga bind qiladi. `Coin::Quarter(state)` patterni enum variantini ham, payloadni ham tekshiradi.

Patterns `match`, [[if-let|if let]], [[while-let|while let]], va [[let-else|let...else]]da ishlatiladi. `match` barcha casesni yopishni talab qiladi; `if let` va `let...else` bitta pattern atrofida concise flow beradi. Pattern familyasiga tuple/struct destructuring, array patterns, slice patterns, guardlar, va `ref` orqali borrow capture ham kiradi.

Declarative macro patternlari bilan buni aralashtirmaslik kerak: `macro_rules!` patternlari runtime value'ni emas, syntax fragmentlarini match qiladi.

Patternlar [[irrefutable-pattern|irrefutable]] (har doim mos keladi) yoki [[refutable-pattern|refutable]] (ba'zi qiymatlar uchun mos kelmaydi) bo'lishi mumkin. `let`, `fn` parametrlari, `for` faqat irrefutable; `if let`, `while let`, `let...else` refutable ham qabul qiladi.

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
- [[while-let|while let]]
- [[let-else|let...else]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[refutable-pattern|refutable pattern]]
- [[pattern-destructuring|pattern destructuring]]
- [[or-pattern|or-pattern (`|`)]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[slice-patterns|slice patterns]]
- [[ref-pattern|ref pattern]]
- [[option|Option]]
- [[enums]]
- [[result|Result]]
- [[ordering|Ordering]]
- [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/sources/0-2-introduction]]
- [[6-2-the-match-control-flow-construct]]
- [[6-3-concise-control-flow-with-if-let-and-let-else]]
- [[wiki/sources/19-patterns-and-matching]]
- [[wiki/sources/19-1-all-the-places-patterns-can-be-used]]
- [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match]]
- [[wiki/sources/19-3-pattern-syntax]]
- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
