---
title: "let...else"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, control-flow, pattern-matching]
source_count: 1
---

# let...else

## Short Definition

`let...else` — pattern mos kelsa bindingni outer scope'da yaratadigan, mos kelmasa `else` branch orqali erta chiqishni talab qiladigan Rust syntax.

## Why It Matters

Nested `if let` asosiy ishni ichki blocklarga chuqur surib yuboradi. `let...else` "kerakli shape bo'lmasa, early return; mos kelsa, happy pathni davom ettir" flowini aniqroq qiladi.

## Mental Model

`let PATTERN = EXPRESSION else { ... };`ni guard sifatida ko'ring:

- Pattern mos kelsa, pattern ichidagi bindinglar keyingi outer scope'da mavjud bo'ladi.
- Pattern mos kelmasa, `else` block bajariladi va u normal davom eta olmaydi; odatda `return`, `break`, `continue`, yoki `panic!` bilan control flowdan chiqadi.

## Syntax and Examples

```rust
fn describe_state_quarter(coin: Coin) -> Option<String> {
    let Coin::Quarter(state) = coin else {
        return None;
    };

    if state.existed_in(1900) {
        Some(format!("{state:?} is pretty old, for America!"))
    } else {
        Some(format!("{state:?} is relatively new."))
    }
}
```

`Option<T>` bilan:

```rust
fn print_name(name: Option<String>) {
    let Some(name) = name else {
        return;
    };

    println!("{name}");
}
```

## Common Mistakes

- `else` blockda `return`/`break`/`continue`/`panic!` qilmaslik; `let...else`ning `else` arm'i normal value bilan davom eta olmaydi.
- `let...else` statement oxiridagi semicolonni unutish.
- Oddiy ikki branchli logic uchun `let...else` ishlatib, `match` yoki `if let else` aniqroq bo'ladigan joyni murakkablashtirish.

## Related Concepts

- [[if-let|if let]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[option|Option]]
- [[enums]]
- [[control-flow|control flow]]

## Sources

- [[6-3-concise-control-flow-with-if-let-and-let-else]]

