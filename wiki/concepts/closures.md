---
title: "Closures"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, closures, fn-traits, capture, functional]
source_count: 8
---

# Closures

## Short Definition

`|params| body` shaklida yoziladigan anonim funksiya. O'zgaruvchida saqlanishi yoki boshqa funksiyaga argument sifatida uzatilishi mumkin. Oddiy funksiyadan farqi: **aniqlangan scope'dagi qiymatlarni capture qila oladi**.

## Why It Matters

Closures iterator metodlari (`map`, `filter`, `filter_map`, `sort_by_key`), optional qiymatlar (`unwrap_or_else`, `Option::map`, `Option::and_then`), `Result` composition (`map`, `and_then`), va multithreading (`thread::spawn`) uchun asosiy building block. Capture qilish imkoniyati closure'ni funksiyadan ancha moslashuvchan qiladi.

## Mental Model

Closure — funksiya + "xotira": u aniqlangan joydan zarur qiymatlarni olib, keyinchalik boshqa joyda bajarilganida ham ulardan foydalana oladi. Xuddi lambdalar kabi (Python, JavaScript, Java), lekin ownership qoidalariga to'liq bo'ysunadi.

Muhim chegara: non-capturing anonymous function ko'pincha [[function-pointers|function pointer]]ga koerce bo'lishi mumkin, capturing closure esa alohida compiler-generated object bo'ladi va `Fn*` traitlaridan birini implement qiladi.

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
// Non-capturing closure function pointerga koerce bo'ladi
let inc: fn(i32) -> i32 = |x| x + 1;
```

```rust
// Argument sifatida uzatish
let result = Some(ShirtColor::Blue)
    .unwrap_or_else(|| ShirtColor::Red);
```

```rust
let next = Some(5).map(|x| x + 1);
let chained = Ok::<i32, String>(2).map(|x| x * 2);
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

**4. `move` har doim majburiy deb o'ylash:**
Local scope ichida ko'p closure'larda compiler capture usulini o'zi tanlaydi. `move` ayniqsa returned closure, thread, yoki ownershipni ataylab ichkariga o'tkazish kerak bo'lganda muhim.

## Thread kontekstida `move`

`thread::spawn(move || ...)` — eng keng tarqalgan `move` closure ishlatilishi. Closure parent thread'dan qiymat ishlatishi kerak bo'lganda `move` majburiy, chunki Rust thread lifetime'ni bila olmaydi:

```rust
use std::thread;

let v = vec![1, 2, 3];
// move: v ownership thread'ga o'tadi
let handle = thread::spawn(move || println!("{v:?}"));
handle.join().unwrap();
```

Batafsil: [[move-closures-threads]].

## Returning Closures

Functiondan closure qaytarishda closure'ning concrete type nomini yozib bo'lmaydi. Bitta closure implementation uchun:

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

Turli closure typelarini bitta collectionda saqlash kerak bo'lsa, [[trait-object|trait object]] ishlatiladi:

```rust
fn handler(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}
```

Batafsil: [[returning-closures]] va [[opaque-types]].

## Related Concepts

- [[fn-traits]] (`FnOnce`, `FnMut`, `Fn`) — closure'ning chaqirilish imkoniyatlari
- [[returning-closures]] — `impl Fn` va `Box<dyn Fn>`
- [[function-pointers]] — named functionlar closure traitlarini implement qiladi
- [[iterators]] — ko'p iterator metodlari closure qabul qiladi
- [[move-semantics]] — `move` keyword
- [[borrowing]] — capture modlari borrow qoidalariga bo'ysunadi
- [[type-inference]] — closure type annotation aksariyat shart emas
- [[concurrency]] — `move` closures thread'lar uchun zarur
- [[move-closures-threads]] — thread kontekstida `move` closure

## Sources

- [[13-1-closures|13.1 Closures]]
- [[13-functional-language-features-iterators-and-closures|13. Intro]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
- [[wiki/sources/rust-for-backend-developers-option]]
- [[wiki/sources/rust-for-backend-developers-result]]
- [[wiki/sources/rust-for-backend-developers-iterators]]
