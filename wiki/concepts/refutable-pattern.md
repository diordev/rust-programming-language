---
title: "Refutable Pattern"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, refutability]
source_count: 1
---

# Refutable Pattern

## Short Definition

Ba'zi qiymatlar uchun mos kelmasligi mumkin bo'lgan pattern — muvaffaqiyatsizlik holati mavjud.

## Why It Matters

`if let`, `while let`, `let...else` va `match` armlar refutable patternlarni kutadi, chunki bu konstruksiyalar "mos kelmasa nima qilish kerak" holatini handle qila oladi.

Teskari — [[irrefutable-pattern|irrefutable pattern]]: har doim muvaffaqiyatli.

## Mental Model

"Bu pattern ba'zi qiymatlar bilan mos kelmaydi." Masalan `Some(x)` — agar qiymat `None` bo'lsa muvaffaqiyatsiz. Compiler bu pattern'ni faqat muvaffaqiyatsizlik holati handle qilingan joylarda qabul qiladi.

## Syntax and Examples

```rust
// Refutable patternlar:
Some(x)         // None bo'lsa mos kelmaydi
Ok(val)         // Err bo'lsa mos kelmaydi
Err(e)          // Ok bo'lsa mos kelmaydi
Point { x: 0 }  // x 0 bo'lmasa mos kelmaydi
```

To'g'ri kontekstlarda ishlatish:

```rust
// if let — to'g'ri
if let Some(x) = maybe_value {
    println!("{x}");
}

// while let — to'g'ri
while let Ok(v) = rx.recv() {
    println!("{v}");
}

// let...else — to'g'ri
let Some(x) = maybe_value else {
    return;
};
```

Noto'g'ri: refutable pattern `let`da:

```rust
let Some(x) = maybe_value;
// error[E0005]: refutable pattern in local binding
// `let` bindings require an "irrefutable pattern"
```

## Common Mistakes

- `let`da refutable pattern ishlatish → `E0005`.
- `if let`da irrefutable pattern ishlatish → compiler warning (doim true, `else` kerak emas).

## Related Concepts

- [[irrefutable-pattern|irrefutable pattern]]
- [[pattern-matching|pattern matching]]
- [[let-else|let...else]]
- [[if-let|if let]]
- [[while-let|while let]]
- [[option|Option]]
- [[result|Result]]
- [[e0005-refutable-pattern-in-local-binding|E0005]]

## Sources

- [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match]]
