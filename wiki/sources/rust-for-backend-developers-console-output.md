---
title: "Печать на консоль - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, formatting]
source_count: 1
---

# Печать на консоль - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/10. Печать на консоль.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/console-output.html

## Detailed Summary

Bu source `println!`ni faqat "text chiqarish" emas, balki Rustdagi formatting modeliga kirish nuqtasi sifatida ko'rsatadi. Eng birinchi tuzatish: `println!` function emas, macro.

Source oddiy text printingdan boshlanadi, keyin `{}` placeholders orqali value chiqarishni ko'rsatadi. Bir nechta value bo'lsa, placeholderlar soni argumentlar soniga mos bo'lishi kerak. Shundan keyin ikki qulay syntax beriladi: inline capture (`println!("Number is {magic_number}")`) va named arguments (`num = ...`, `square = ...`).

Keyingi muhim boundary `Display` va `Debug` orasida. `{}` faqat `std::fmt::Display` implement qilingan typelar bilan ishlaydi. Primitive typelar uchun bu odatda muammo emas, lekin masalan array kabi ko'p typelar `Display`ni implement qilmaydi. Shunda `{:?}` ishlatiladi, bu esa `Debug` formattingga o'tadi.

Source hozircha macro implementation ichiga kirmaydi; bu keyingi macro chapterlarga qoldiriladi. Lekin amaliy natija allaqachon muhim: user-facing output uchun `{}`, developer-facing inspection uchun ko'pincha `{:?}`.

## Key Concepts

- [[println-macro|println! macro]]
- [[display-formatting|Display formatting]]
- [[debug-formatting|Debug formatting]]
- [[debug-trait|Debug trait]]
- [[macro]]
- [[stdout]]

## Code Examples

```rust
println!("Print just text");
```

```rust
let magic_number: i32 = 5;
println!("Number is {}", magic_number);
println!("Number is {magic_number}");
```

```rust
println!(
    "Number is {num}, num squared is {square}",
    num = magic_number,
    square = magic_number * magic_number,
);
```

```rust
let arr = [1, 2, 3];
println!("{arr:?}");
```

## Exercises or Practice Ideas

- Bitta `println!` ichida positional, inline capture, va named argument variantlarini yozib chiqing.
- Arrayni `{}` bilan chiqarishga urining, keyin `{:?}`ga o'tib farqini kuzating.
- User-facing va debug output uchun ikki xil format satri yozing.

## Questions Raised

- Qachon `Display` implement qilish kerak, qachon `Debug` yetarli?
- `println!` bilan `format!` orasidagi boundary qayerda?
- Named arguments uzun format satrlarida readability'ni qanchalik oshiradi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-console-output]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[println-macro|println! macro]]
- [[display-formatting|Display formatting]]
- [[debug-formatting|Debug formatting]]
