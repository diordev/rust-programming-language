---
title: "Циклы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, loops]
source_count: 1
---

# Циклы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/18. Циклы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/loops.html

## Detailed Summary

Bu source Rustdagi uch asosiy loop formasini bir joyga yig'adi: `while`, `loop`, va `for`. Shu bilan birga Rustda C-style classic `for(init; cond; step)` yo'qligini aniq aytadi.

`while` boshqa imperative tillardagidek condition-checked loop. Source countdown misolini beradi. Qiziq tomoni, `do-while` Rustda alohida syntax emas; kerak bo'lsa `while { ... condition ... } {}` shakliga yaqin trick bilan imitatsiya qilinishi mumkin. Bu ko'proq curiosity darajasidagi material.

`loop` special infinite loop. Uni oddiy `while true` analogi deb tushuntirish mumkin, lekin amaliy farqi muhim: `break value` bilan qiymat qaytara oladi. Source `counter == 10` bo'lganda `break counter * 2` misolini beradi va loop expression natijasi variable'ga assign qilinadi.

`for` Rustda for-each style. Arrays, slices, vectors, ranges va boshqa iterable narsalar ustida yuradi. Source C-style for yo'qligini aytib, numeric iteration uchun `0..10` va `0..=10` rangesni ko'rsatadi.

Bu chapter keyingi ownership va iterators materialiga tayanch bo'ladi, chunki `for` aslida iterable protocol bilan ishlaydi.

## Key Concepts

- [[while-loop|while loop]]
- [[loop]]
- [[for-loop|for loop]]
- [[range]]
- [[break]]
- [[continue]]
- [[never-type|never type (!)]]

## Code Examples

```rust
let mut n = 5;
while n > 0 {
    println!("{n}");
    n -= 1;
}
```

```rust
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};
```

```rust
for i in 0..10 {
    print!("{i}, ");
}

for i in 0..=10 {
    print!("{i}, ");
}
```

## Exercises or Practice Ideas

- Bitta vazifani `while`, `loop`, va `for` bilan alohida yozing.
- `break value` bilan loop expression ishlatadigan misol yozing.
- `0..10` va `0..=10` farqini bitta test output bilan ko'rsating.

## Questions Raised

- `loop` qachon `while`dan tozaroq ko'rinadi?
- `for`ning iterator modelini ownership chapterlarisiz qanchalik erta tushuntirish kerak?
- `do-while` imitation real code'da deyarli kerak bo'ladimi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-loops]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[while-loop|while loop]]
- [[loop]]
- [[for-loop|for loop]]
