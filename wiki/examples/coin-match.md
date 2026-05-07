---
title: "Coin match example"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, match, enums]
source_count: 1
---

# Coin match example

## Goal

[[enums]] va [[match]] birga ishlaganda enum variantlariga qarab value qaytarish va variant ichidagi data'ni bind qilishni ko'rsatish.

## Code

```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {state:?}!");
            25
        }
    }
}

fn main() {
    let cents = value_in_cents(Coin::Quarter(UsState::Alaska));
    println!("{cents}");
}
```

## Explanation

`Coin` enumida `Quarter` variant payload sifatida `UsState` tutadi. `match` armidagi `Coin::Quarter(state)` patterni faqat quarter qiymatiga mos keladi va ichki `UsState` value'ni `state` nomi bilan bind qiladi.

Har bir arm `u8` qaytaradi, shuning uchun butun `match` expression'i ham `u8` qaytaradi. Agar `Coin`ga yangi variant qo'shilsa, [[exhaustive-matching|exhaustive matching]] sabab compiler `value_in_cents`dagi `match`ni yangilashni talab qiladi.

## Related Pages

- [[6-2-the-match-control-flow-construct-the-rust-programming-language]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[exhaustive-matching|exhaustive matching]]
