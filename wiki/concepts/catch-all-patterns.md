---
title: "Catch-All Patterns"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, match, pattern-matching]
source_count: 2
---

# Catch-All Patterns

## Short Definition

Catch-all pattern — `match`da oldingi patternsga mos kelmagan barcha qiymatlarni qamrab oladigan oxirgi arm.

## Why It Matters

[[exhaustive-matching|Exhaustive matching]] talabini bajarishda ba'zan barcha qolgan cases uchun bitta default behavior kerak bo'ladi. Catch-all pattern buni aniq ifodalaydi.

## Mental Model

Patterns yuqoridan pastga tekshiriladi. Catch-all "shu paytgacha mos kelmagan hammasi" degani, shuning uchun odatda oxirgi arm bo'ladi.

- Named binding (`other`) qolgan qiymatni ishlatish uchun kerak.
- `_` qolgan qiymat kerak emasligini bildiradi va unused variable warning bermaydi.
- `_ => ()` qolgan casesda explicit "hech narsa qilma" degani.

## Syntax and Examples

Qolgan qiymatni ishlatish:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),
}
```

Qolgan qiymatni ignore qilish:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => reroll(),
}
```

Qolgan qiymatlar uchun hech narsa qilmaslik:

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => (),
}
```

## `_name` — Bind qiladi, warning bermaydi

`_x` bilan `_` o'rtasidagi farq muhim:
- `_x` — qiymatni bind **qiladi** (move ham qilishi mumkin!)
- `_` — bind **qilmaydi**

```rust
let s = Some(String::from("Hello!"));
if let Some(_s) = s { ... }   // s moved! Keyin foydalanib bo'lmaydi
// println!("{s:?}");          // XATO

let s = Some(String::from("Hello!"));
if let Some(_) = s { ... }    // move yo'q
println!("{s:?}");             // OK: Some("Hello!")
```

## `..` — Qolganlarni ignore

Struct fieldlarini o'tkazib yuborish:
```rust
match origin {
    Point { x, .. } => println!("x = {x}"),  // y, z ignore
}
```

Tuple'ning o'rtasini o'tkazib yuborish:
```rust
match numbers {
    (first, .., last) => println!("{first}, {last}"),
}
```

`..` noaniq bo'lmasligi kerak:
```rust
(.., second, ..) => ...  // XATO: compiler bilmaydi second qayerda
```

## Common Mistakes

- Catch-all armni yuqoriga qo'yish; keyingi arm'lar unreachable bo'ladi.
- `_` ishlatib, keyin default value kerak bo'lib qolganini sezmaslik; bunday paytda named binding kerak.
- `_` bilan yangi enum variants compiler tomonidan eslatilishini yo'qotish.
- `_name` ishlatganda ownership'ga e'tibor bermaslik — `String` kabi non-`Copy` typelar moved bo'ladi.
- `..` ni tuple'da ikki marta ishlatish → compile xatosi.

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[unit-type|unit type]]
- [[pattern-destructuring|pattern destructuring]]

## Sources

- [[6-2-the-match-control-flow-construct]]
- [[wiki/sources/19-3-pattern-syntax]]

