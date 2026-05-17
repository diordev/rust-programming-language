---
title: "Chapter 19.2: Refutability — Whether a Pattern Might Fail to Match"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, refutability, irrefutable, refutable]
source_count: 1
---

# Chapter 19.2: Refutability — Whether a Pattern Might Fail to Match

## Learning Goal

Irrefutable va refutable patternlar orasidagi farqni tushunish va qaysi konstruksiyada qaysi tur kerakligini bilish.

## Main Ideas

### Ikki turdagi pattern

**Irrefutable:** Har qanday qiymatga mos keladi.
```rust
let x = 5;  // x — irrefutable
```

**Refutable:** Ba'zi qiymatlar uchun mos kelmaydi.
```rust
if let Some(x) = value  // Some(x) — refutable (None bo'lsa mos kelmaydi)
```

### Kontekst qoidalari

| Konstruksiya | Kerakli pattern | Sababi |
|---|---|---|
| `fn` parametrlari | Irrefutable | Mos kelmaslik holati yo'q |
| `let` | Irrefutable | Mos kelmaslik holati yo'q |
| `for` | Irrefutable | Mos kelmaslik holati yo'q |
| `if let` | Ikkisi (irrefutable → warning) | Failure holati handle qilingan |
| `while let` | Ikkisi (irrefutable → warning) | Failure holati handle qilingan |
| `let...else` | Ikkisi (irrefutable → warning) | `else` failure handle qiladi |
| `match` armlar | Oxirgi — irrefutable yoki barcha cover | Exhaustive kerak |

### Xato holat: refutable let da

```rust
let Some(x) = maybe_value;  // XATO: E0005
// error: refutable pattern in local binding
// pattern `None` not covered
```

Compiler o'zi `let...else` ishlatishni tavsiya qiladi.

### To'g'ri yechim: let...else

```rust
let Some(x) = maybe_value else {
    return;  // yoki break, continue, panic! ...
};
// x bu yerda outer scope da mavjud
```

### Ogohlantirish: irrefutable let...else da

```rust
let x = 5 else {
    return;
};
// warning: irrefutable `let...else` pattern
// this pattern will always match, so `else` is useless
```

## Concepts

- [[irrefutable-pattern|irrefutable pattern]]
- [[refutable-pattern|refutable pattern]]
- [[let-else|let...else]]
- [[if-let|if let]]
- [[while-let|while let]]
- [[pattern-matching|pattern matching]]

## Examples

```rust
// Irrefutable — to'g'ri joy
let x = 5;
let (a, b) = (1, 2);
fn foo(x: i32) { ... }

// Refutable — if let bilan to'g'ri
if let Some(x) = maybe { println!("{x}"); }

// Refutable — let...else bilan to'g'ri
let Some(x) = maybe else { return; };

// XATO: refutable let da
// let Some(x) = maybe;  // E0005
```

## Exercises

1. Quyidagi kodni to'g'rilang:
   ```rust
   let Some(name) = get_user_name();
   ```
2. `let...else` bilan `Option<i32>` qiymatini outer scope ga chiqaring.
3. `if let` o'rniga `let...else` ishlatib funksiyadan erta chiqishni yozing.

## Review Questions

1. Irrefutable pattern nima? Misol keltiring.
2. Refutable pattern nima? Misol keltiring.
3. `let Some(x) = value;` nima uchun xato?
4. `let...else` da `else` blokidan qanday chiqiladi?
5. `if let x = 5` nima uchun ogohlantirish beradi?

## Related Pages

- [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match]]
- [[wiki/chapters/19-patterns-and-matching]]
- [[wiki/chapters/19-1-all-the-places-patterns-can-be-used]]
- [[irrefutable-pattern]]
- [[refutable-pattern]]
- [[let-else]]
