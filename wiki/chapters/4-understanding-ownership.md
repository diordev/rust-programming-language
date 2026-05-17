---
title: "4. Understanding Ownership"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, ownership]
source_count: 4
---

# 4. Understanding Ownership

## Learning Goal

Rust memory safety modelining asosiy mental modelini tushunish: owner, scope, move, clone, Copy, Drop, stack/heap, borrowing, references, va slices.

## Main Ideas

- Ownership Rustning garbage collector'siz memory safety beradigan asosiy sistemi.
- Heap data cleanup'i owner scope'dan chiqqanda `drop` orqali deterministik bajariladi.
- Har bir value bitta ownerga ega; owner scope'dan chiqqanda value dropped bo'ladi.
- `String` heap-backed value bo'lgani uchun assignmentda move bo'ladi.
- Move stack metadata copy qiladi, heap data copy qilmaydi, va oldingi bindingni invalid qiladi.
- `clone()` explicit deep copy signalidir.
- `Copy` traitli stack-only types assignmentda move bo'lmaydi, trivially copied bo'ladi.
- Function parameters va return values ownershipni assignment kabi transfer qiladi.
- [[borrowing]] value'dan ownershipni olmasdan foydalanish imkonini beradi.
- Bir vaqtda yoki bitta [[mutable-reference|mutable reference]], yoki istalgancha immutable references bo'lishi mumkin.
- [[slices]] collection ichidagi contiguous qismga ownershipsiz reference beradi.
- `&str` parameter `&String`dan ko'ra flexible API yaratadi.

## Concepts

- [[ownership]]
- [[stack-and-heap|stack and heap]]
- [[scope]]
- [[string-type|String]]
- [[drop]]
- [[move-semantics|move semantics]]
- [[clone]]
- [[copy-trait|Copy trait]]
- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[slices]]
- [[string-slice|String slice]]

## Examples

Move:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

Clone:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```

Copy:

```rust
let x = 5;
let y = x;
println!("x = {x}, y = {y}");
```

Borrow:

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Slice:

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

## Exercises

- `String` move exampleida oldingi bindingni ishlatib `E0382`ni ko'rish.
- `clone()` bilan xatoni tuzatish.
- Functionga `String` uzatib, call'dan keyin variable invalid bo'lishini tekshirish.
- Functionga `i32` uzatib, call'dan keyin variable valid qolishini tekshirish.
- Ikki simultaneous `&mut` reference yaratib compiler errorni ko'rish.
- `first_word`ni `usize` va `&str` return versions bilan yozib API safety farqini solishtirish.

## Review Questions

- Ownershipning 3 rule'i nima?
- Stack va heap o'rtasidagi performance/structure farqi nima?
- `String` assignmentda nima uchun move bo'ladi?
- `clone()` nima uchun explicit bo'lishi kerak?
- `Copy` trait qaysi turdagi typelarga mos?
- Function parameteriga `String` berilganda ownership qayerga o'tadi?
- Reference nima uchun value'ni drop qilmaydi?
- Mutable reference restriction data racesni qanday oldini oladi?
- `&str` nima uchun `&String`dan idiomaticroq parameter type?

## Related Pages

- [[wiki/sources/4-understanding-ownership]]
- [[4-1-what-is-ownership]]
- [[4-2-references-and-borrowing]]
- [[4-3-the-slice-type]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[move-vs-clone]]
- [[string-literal-vs-string]]
- [[rust-learning-roadmap]]
