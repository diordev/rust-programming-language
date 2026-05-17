---
title: "Условный оператор if - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, control-flow]
source_count: 1
---

# Условный оператор if - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/17. Условный оператор if.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/operator-if.html

## Detailed Summary

Bu source Rustdagi `if`ni oddiy control-flow statement emas, balki expression sifatida taqdim etadi. Syntactic jihatdan tanish, lekin semantik farq muhim.

Branch body har doim curly braces bilan yoziladi. C yoki Java'dagi bitta statement uchun braces tashlab yuborish Rustda yo'q. Bu syntactic qat'iylik if'ni block expression sifatida ko'rishni osonlashtiradi.

Source'ning markaziy gapi: `if` value qaytaradi. Ishlagan branchdagi oxirgi expression butun `if` expression natijasiga aylanadi. Shu sabab `if`ni variable assignment ichida ishlatish mumkin.

Semicolon bu yerda yana hal qiluvchi rol o'ynaydi. Agar branch ichidagi oxirgi expressiondan keyin `;` qo'yilsa, branch result `()`ga aylanadi. Source buni `let mod_a: () = if ...` misoli bilan ko'rsatadi. Bu `if` semanticsini [[scope]] va [[statements-and-expressions|statements and expressions]] bilan to'g'ridan-to'g'ri bog'laydi.

## Key Concepts

- [[if-expressions|if expressions]]
- [[statements-and-expressions|statements and expressions]]
- [[scope]]
- [[unit-type|unit type]]
- [[boolean-type|bool]]

## Code Examples

```rust
if a % 2 == 0 {
    println!("a is even");
} else {
    println!("a is odd");
}
```

```rust
let mod_a: i32 = if a < 0 { -a } else { a };
```

```rust
let mod_a: () = if a < 0 { -a; } else { a; };
println!("{mod_a:?}");
```

## Exercises or Practice Ideas

- `if`ni expression sifatida ishlatadigan 3 ta misol yozing.
- `;` qo'yilgan va qo'yilmagan branchlar typesini solishtiring.
- Uzoq `else if` zanjiri qachon `match`ga aylanishi kerakligini yozing.

## Questions Raised

- `if` expressionni `match`dan qachon afzal ko'rish kerak?
- Har branch bir xil type qaytarish talabi qaysi xatolarni erta ushlaydi?
- `()`ga aylangan branchlar real loyihada qaysi type mismatch'larni keltirib chiqaradi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-if]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[if-expressions|if expressions]]
- [[statements-and-expressions|statements and expressions]]
