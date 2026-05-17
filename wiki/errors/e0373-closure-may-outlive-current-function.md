---
title: "E0373: closure may outlive current function"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, compiler-error, closures, threads, lifetimes]
source_count: 1
---

# E0373: closure may outlive current function

## Symptom

```
error[E0373]: closure may outlive the current function, but it borrows `v`,
              which is owned by the current function
```

## Cause

`thread::spawn`ga uzatilgan closure tashqi funksiyaning qiymatini **borrow** qilmoqda. `thread::spawn` `'static` lifetime talab qiladi — ya'ni closure hayot siklida ishlatiladigan barcha referenslar `'static` bo'lishi kerak. Lekin funksiya local o'zgaruvchilar `'static` emas, shuning uchun Rust borrow'ni rad etadi.

Rust thread qancha umr ko'rishini kompilyatsiya vaqtida bila olmaydi — main funksiya tugashi mumkin, spawned thread esa hali ishlayotgan bo'lishi mumkin.

## Fix Pattern

`move` kalit so'zini closure oldiga qo'ying — borrow o'rniga **ownership** o'tkaziladi:

```rust
thread::spawn(move || { /* ... */ });
```

## Minimal Example

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    // XATO: E0373
    let handle = thread::spawn(|| {
        println!("{v:?}");
    });

    // TO'G'RI: move bilan ownership o'tkazish
    let handle = thread::spawn(move || {
        println!("{v:?}");
    });

    handle.join().unwrap();
}
```

## Related Concepts

- [[move-closures-threads]]
- [[closures]]
- [[threads]]
- [[static-lifetime]]
- [[ownership]]
- [[e0382-borrow-of-moved-value]]

## Sources

- [[16-1-using-threads-to-run-code-simultaneously]]
