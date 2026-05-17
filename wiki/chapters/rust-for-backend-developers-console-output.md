---
title: "Rust for Backend Developers: Console Output"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, formatting, chapter]
source_count: 1
---

# Rust for Backend Developers: Console Output

## Learning Goal

`println!` macro, basic formatting placeholders, va `Display`/`Debug` boundarysini amaliy darajada tushunish.

## Main Ideas

- `println!` function emas, macro.
- `{}` user-facing formatting uchun, `{:?}` debug inspection uchun.
- Inline capture va named arguments format satrini o'qiladigan qiladi.
- Har bir type `{}` bilan chiqmaydi; `Display` talab qilinadi.

## Concepts

- [[println-macro|println! macro]]
- [[display-formatting|Display formatting]]
- [[debug-formatting|Debug formatting]]
- [[debug-trait|Debug trait]]

## Examples

```rust
println!("Number is {magic_number}");
println!("Array is {arr:?}");
```

## Exercises

- Bitta format satrida positional va named arguments ishlating.
- Arrayni `{}` bilan chiqarish nega ishlamasligini ko'rsating.

## Review Questions

- `Display` va `Debug` orasidagi amaliy farq nima?
- Qachon `{:?}` tezroq va to'g'riroq tanlov bo'ladi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-console-output]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[println-macro|println! macro]]
- [[display-formatting|Display formatting]]
