---
title: "Display Formatting"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, formatting]
source_count: 4
---

# Display Formatting

## Short Definition

Display formatting `{}` placeholder orqali user-facing output chiqarish formatidir.

## Why It Matters

Primitive typelar ko'pincha `Display` implement qiladi, lekin custom [[structs]] uchun Rust outputni taxmin qilmaydi. Shu sababli `println!("{rect1}")` `Display` implementation talab qiladi.

## Mental Model

`Display` "foydalanuvchi ko'radigan matn" uchun. Custom type uchun bunday matn qanday bo'lishi kerakligini type author belgilashi kerak.

`to_string()` methodi `Display` implement qilgan typelarda mavjud. String literals uchun `"text".to_string()` `String` yaratadi.

Chapter 10.2da `Display` trait bound sifatida ishlatiladi: `T: Summary + Display` function body'ga `{}` formatting imkonini beradi. Standard library blanket implementation orqali `Display` implement qilgan har qanday `T` uchun `ToString`ni beradi.

## Syntax and Examples

```rust
println!("number is {}", 42);
```

Inline capture va named arguments ham baribir `Display` formattingning o'zi:

```rust
let magic_number = 5;
println!("Number is {magic_number}");
println!("Number is {num}", num = magic_number);
```

```rust
let s = "initial contents".to_string();
```

Custom struct uchun bu ishlamasligi mumkin:

```rust
println!("rect1 is {rect1}");
```

Array kabi ko'p standard typelar `Display`ni bermaydi. Bunday paytda `{:?}` bilan [[debug-formatting|Debug formatting]] ishlatiladi.

Trait bound:

```rust
pub fn notify<T: Summary + Display>(item: &T) {
    println!("{}", item);
}
```

## Common Mistakes

- Custom struct `Display`ni avtomatik implement qiladi deb o'ylash.
- Debugging uchun `{}` ishlatish; ko'pincha `{:?}` va [[debug-trait|Debug trait]] kerak.
- `to_string()` har qanday typeda bor deb o'ylash; u `Display`/`ToString` capability bilan bog'liq.
- `Display` bound bo'lmasa generic `T`ni `{}` bilan formatlab bo'ladi deb o'ylash.

## Related Concepts

- [[debug-formatting|Debug formatting]]
- [[debug-trait|Debug trait]]
- [[println-macro|println! macro]]
- [[traits]]
- [[trait-bounds|trait bounds]]
- [[blanket-implementations|blanket implementations]]
- [[conditional-method-implementations|conditional method implementations]]
- [[string-type|String]]

## Sources

- [[5-2-an-example-program-using-structs]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/rust-for-backend-developers-console-output]]
