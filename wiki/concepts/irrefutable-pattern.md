---
title: "Irrefutable Pattern"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, refutability]
source_count: 1
---

# Irrefutable Pattern

## Short Definition

Har qanday mumkin bo'lgan qiymatga mos keladigan pattern — hech qachon muvaffaqiyatsiz bo'lmaydi.

## Why It Matters

`let`, `fn` parametrlari, va `for` looplari faqat irrefutable patternlarni qabul qiladi, chunki bu kontekstlarda mos kelmaslik holati hech qanday manfaatli narsa qila olmaydi (dastur o'sha qiymatga kelib qoladi, lekin nimani qilishni bilmaydi).

## Mental Model

"Bu pattern har doim o'tadi" — hech qanday qiymat uni shikastlay olmaydi. Oddiy o'zgaruvchi nomi (`x`, `name`) har qanday qiymatni qabul qiladi, shuning uchun irrefutable.

Teskari — [[refutable-pattern|refutable pattern]]: ba'zi qiymatlar uchun mos kelmaydi.

## Syntax and Examples

```rust
// Irrefutable patternlar:
let x = 5;               // x — istalgan qiymat
let (a, b) = (1, 2);    // tuple destructuring — element soni mos bo'lsa
fn foo(x: i32) { ... }  // parametr

for val in vec![1, 2, 3] { ... }  // val — irrefutable
```

`let...else`da irrefutable pattern ishlatilsa compiler ogohlantiradi:
```rust
let x = 5 else {
    return;
};
// warning: irrefutable `let...else` pattern
// this pattern will always match, so `else` is useless
```

## Common Mistakes

- `let Some(x) = value;` yozish — `Some(x)` refutable, `let` esa irrefutable talab qiladi → `E0005`.
- `if let x = value` yozish — `x` irrefutable, `if let` esa refutable kutadi → compiler warning.

## Related Concepts

- [[refutable-pattern|refutable pattern]]
- [[pattern-matching|pattern matching]]
- [[let-else|let...else]]
- [[if-let|if let]]

## Sources

- [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match]]
