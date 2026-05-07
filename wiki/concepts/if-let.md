---
title: "if let"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, control-flow, pattern-matching]
source_count: 1
---

# if let

## Short Definition

`if let` — bitta pattern mos kelganda code bajarish, qolgan casesni ignore qilish yoki `else`ga yuborish uchun ishlatiladigan concise [[pattern-matching|pattern matching]] syntax.

## Why It Matters

`match` barcha casesni handle qilishni talab qiladi. Ba'zan esa faqat bitta case qiziq: masalan `Some(value)` bo'lsa ishlash, `None` bo'lsa hech narsa qilmaslik. `if let` shu holatda boilerplateni kamaytiradi.

## Mental Model

`if let PATTERN = EXPRESSION { ... }`ni "agar expression shu pattern shaklida bo'lsa, ichidagi bindinglarni block ichida ishlat" deb o'qing. Bu `match`ning bitta interesting arm va qolgan cases uchun `_ => ()` yoki `else` arm bilan yozilgan qisqa shakli.

Trade-off: `if let` [[exhaustive-matching|exhaustive checking]] bermaydi. Hamma variantlar muhim bo'lsa, [[match]] ishlating.

## Syntax and Examples

```rust
let config_max = Some(3u8);

if let Some(max) = config_max {
    println!("The maximum is configured to be {max}");
}
```

`else` bilan:

```rust
if let Coin::Quarter(state) = coin {
    println!("State quarter from {state:?}!");
} else {
    count += 1;
}
```

Equivalent `match`:

```rust
match config_max {
    Some(max) => println!("The maximum is configured to be {max}"),
    _ => (),
}
```

## Common Mistakes

- Hamma cases muhim bo'lgan joyda `if let` ishlatib, muhim variantni unutish.
- `if let` bindingi faqat block ichida yashashini unutish.
- `_` branchda nima bo'lishi kerakligini ongli qaror qilmasdan "qisqa yozish" uchun `if let`ga o'tish.

## Related Concepts

- [[match]]
- [[let-else|let...else]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[option|Option]]
- [[enums]]
- [[control-flow|control flow]]

## Sources

- [[6-3-concise-control-flow-with-if-let-and-let-else-the-rust-programming-language]]

