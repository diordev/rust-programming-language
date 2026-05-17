---
title: "Rust for Backend Developers: Pattern Matching"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, control-flow, chapter]
source_count: 1
---

# Rust for Backend Developers: Pattern Matching

## Learning Goal

`match`ni simple branching emas, pattern-aware expression sifatida tushunish: ranges, guards, bindings, slice patterns, va `ref` bilan borrow capture qayerda ishlashini ko'rish.

## Main Ideas

- `match` patternlarni yuqoridan pastga tekshiradi va birinchi mos armni bajaradi.
- `_`, `|`, va range patternlar branch selectionni ixcham qiladi.
- `@` binding test va capture'ni bitta pattern ichida birlashtiradi.
- String content matchingda bare variable pattern comparison emas, binding bo'lishi mumkin; literal yoki guard ishlatiladi.
- Slice patterns `match` ichida ishlaydi, chunki mismatch keyingi armga o'tadi.
- `ref` va `ref mut` fieldlarni move qilmay, reference bilan ushlashga imkon beradi.

## Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[slice-patterns|slice patterns]]
- [[ref-pattern|ref pattern]]
- [[catch-all-patterns|catch-all patterns]]
- [[exhaustive-matching|exhaustive matching]]

## Examples

```rust
match guess {
    0..=9 => println!("single digit"),
    _ => println!("multi digit"),
}
```

```rust
match bytes {
    [] => 0,
    [head, ..] => *head,
}
```

## Exercises

- `@` binding va range bilan input validator yozing.
- `match`da `ref mut` ishlatib struct fieldini joyida yangilang.

## Review Questions

- `match guard` qachon patternning o'zi yetmaydigan joyni yopadi?
- `match`dagi slice pattern plain destructuringdan nimasi bilan farq qiladi?
- `ref` pattern bilan `match &value` orasida qanday semantic farq bor?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[ref-pattern|ref pattern]]
