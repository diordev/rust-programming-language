---
title: "E0005 refutable pattern in local binding"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, errors, patterns]
source_count: 1
---

# E0005 refutable pattern in local binding

## Symptom

Compiler `let` iborasida [[refutable-pattern|refutable pattern]] ishlatishga urinishini rad etadi:

```
error[E0005]: refutable pattern in local binding
 --> src/main.rs:3:9
  |
3 |     let Some(x) = some_option_value;
  |         ^^^^^^^ pattern `None` not covered
  |
  = note: `let` bindings require an "irrefutable pattern", like a `struct` or an `enum` with only one variant
  help: you might want to use `let else` to handle the variant that isn't matched
```

## Cause

`let PATTERN = EXPRESSION;` faqat [[irrefutable-pattern|irrefutable pattern]]ni qabul qiladi — har qanday qiymatga mos keladigan pattern. `Some(x)` esa refutable: agar qiymat `None` bo'lsa, mos kelmaydi. `let` mos kelmaslik holatini handle qila olmaydi.

Xuddi shu cheklov [[functions|funksiya parametrlari]] va `for` looplariga ham tegishli.

## Fix Pattern

Refutable patternlarni handle qilishning to'g'ri konstruksiyalaridan birini tanlang:

**[[let-else|let...else]]** — pattern mos kelmasa erta chiqish:
```rust
let Some(x) = some_option_value else {
    return;
};
```

**[[if-let|if let]]** — pattern mos kelsa block ichida ishlash:
```rust
if let Some(x) = some_option_value {
    println!("{x}");
}
```

**[[match]]** — har bir variantni explicit handle qilish:
```rust
match some_option_value {
    Some(x) => println!("{x}"),
    None => return,
}
```

## Minimal Example

```rust
fn main() {
    let some_option_value: Option<i32> = None;
    let Some(x) = some_option_value;  // E0005
}
```

To'g'rilash:
```rust
fn main() {
    let some_option_value: Option<i32> = None;
    let Some(x) = some_option_value else {
        return;
    };
}
```

## Related Concepts

- [[refutable-pattern|refutable pattern]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[let-else|let...else]]
- [[if-let|if let]]
- [[pattern-matching|pattern matching]]

## Sources

- [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match]]
