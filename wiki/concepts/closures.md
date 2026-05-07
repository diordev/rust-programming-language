---
title: "Closures"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, closures, fn-traits, capture, functional]
source_count: 1
---

# Closures

## Short Definition

`|params| body` shaklida yoziladigan anonim funksiya. O'zgaruvchida saqlanishi yoki boshqa funksiyaga argument sifatida uzatilishi mumkin. Oddiy funksiyadan farqi: **aniqlangan scope'dagi qiymatlarni capture qila oladi**.

## Why It Matters

Closures iterator metodlari (`map`, `filter`, `sort_by_key`), optional qiymatlar (`unwrap_or_else`), va multithreading (`thread::spawn`) uchun asosiy building block. Capture qilish imkoniyati closure'ni funksiyadan ancha moslashuvchan qiladi.

## Mental Model

Closure — funksiya + "xotira": u aniqlangan joydan zarur qiymatlarni olib, keyinchalik boshqa joyda bajarilganida ham ulardan foydalana oladi. Xuddi lambdalar kabi (Python, JavaScript, Java), lekin ownership qoidalariga to'liq bo'ysunadi.

## Syntax and Examples

```rust
// To'rtta ekvivalent yozuv:
fn  add_one_v1   (x: u32) -> u32 { x + 1 }   // funksiya
let add_one_v2 = |x: u32| -> u32 { x + 1 };  // to'liq annotatsiyali
let add_one_v3 = |x|             { x + 1 };  // inferred
let add_one_v4 = |x|               x + 1  ;  // bir ifoda
```

```rust
// Muhitdan immutable capture
let list = vec![1, 2, 3];
let only_borrows = || println!("{list:?}");
only_borrows();

// Muhitdan mutable capture
let mut list = vec![1, 2, 3];
let mut borrows_mutably = || list.push(7);
borrows_mutably();

// move: ownership o'tkazish (thread uchun)
use std::thread;
let list = vec![1, 2, 3];
thread::spawn(move || println!("{list:?}")).join().unwrap();
```

```rust
// Argument sifatida uzatish
let result = Some(ShirtColor::Blue)
    .unwrap_or_else(|| ShirtColor::Red);
```

## Common Mistakes

**1. Birinchi chaqiruvdan keyin tip qotadi — boshqa tip bilan chaqirib bo'lmaydi:**
```rust
let ex = |x| x;
ex(String::from("a")); // tip = String
ex(5);                  // E0308: mismatched types
```

**2. Mutable borrow ochiq paytda immutable borrow:**
```rust
let mut list = vec![1, 2, 3];
let mut borrow_mut = || list.push(7);
println!("{list:?}"); // E0502: borrow_mut hali active
borrow_mut();
```

**3. `FnMut` kerak joyda `FnOnce` closure:**
```rust
// sort_by_key FnMut talab qiladi
list.sort_by_key(|r| {
    sort_ops.push(value); // value move bo'ladi — E0507
    r.width
});
```

## Related Concepts

- [[fn-traits]] (`FnOnce`, `FnMut`, `Fn`) — closure'ning chaqirilish imkoniyatlari
- [[iterators]] — ko'p iterator metodlari closure qabul qiladi
- [[move-semantics]] — `move` keyword
- [[borrowing]] — capture modlari borrow qoidalariga bo'ysunadi
- [[type-inference]] — closure type annotation aksariyat shart emas
- [[concurrency]] — `move` closures thread'lar uchun zarur

## Sources

- [[13-1-closures-the-rust-programming-language|13.1 Closures]]
- [[13-functional-language-features-the-rust-programming-language|13. Intro]]
