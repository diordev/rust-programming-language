---
title: "String updating and concatenation"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, strings]
source_count: 1
---

# String updating and concatenation

## Goal

`String` yaratish, update qilish, `+` operatorining ownership behaviorini, va [[format-macro|format! macro]] bilan readable concatenationni ko'rsatish.

## Code

```rust
fn main() {
    let mut greeting = String::from("Hello");
    greeting.push_str(", ");
    greeting.push('R');
    greeting.push_str("ust");

    let suffix = String::from("!");
    let message = greeting + &suffix;

    println!("{message}");
    println!("suffix is still valid: {suffix}");

    let a = String::from("tic");
    let b = String::from("tac");
    let c = String::from("toe");
    let joined = format!("{a}-{b}-{c}");

    println!("{joined}");
}
```

## Explanation

`push_str` `&str` oladi va ownershipni olmaydi. `push` bitta `char` qo'shadi. `greeting + &suffix` `greeting`ni move qiladi, lekin `suffix` reference sifatida ishlatilgani uchun valid qoladi.

`format!` `String` qaytaradi va argumentlarning ownershipini olmaydi, shuning uchun ko'p bo'lakli string yaratishda ko'pincha o'qilishi osonroq.

## Related Pages

- [[8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language]]
- [[string-type|String]]
- [[string-concatenation|string concatenation]]
- [[format-macro|format! macro]]
- [[move-semantics|move semantics]]
- [[deref-coercions|deref coercions]]
