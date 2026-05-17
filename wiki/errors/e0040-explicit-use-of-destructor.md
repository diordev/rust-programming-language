---
title: "E0040: explicit use of destructor method"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, error, drop, smart-pointers]
source_count: 1
---

# E0040: explicit use of destructor method

## Symptom

`Drop` trait ning `drop` metodini qo'lda chaqirishga urinildi:

```
error[E0040]: explicit use of destructor method
  --> src/main.rs:16:7
   |
16 |     c.drop();
   |       ^^^^ explicit destructor calls not allowed
   |
help: consider using `drop` function
   |
16 -     c.drop();
16 +     drop(c);
```

## Cause

Rust scope oxirida `drop`ni avtomatik chaqiradi. Agar programmist ham qo'lda chaqirsa, bir qiymat ikki marta tozalanadi — **double free** xatosi. Rust bu muammoni compile time da oldini oladi.

## Fix Pattern

`Drop::drop` metodini chaqirishga urinmang. Erta tozalash kerak bo'lsa, `std::mem::drop` funksiyasini (prelude da) ishlating:

```rust
// Noto'g'ri
c.drop();

// To'g'ri — erta tozalash kerak bo'lsa
drop(c);
```

## Minimal Example

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping!");
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("data") };
    c.drop();   // E0040
    // drop(c); // to'g'ri variant
}
```

## Related Concepts

- [[drop|Drop trait]]
- [[drop]] — avtomatik cleanup mexanizmi
- [[smart-pointers]]

## Sources

- [[15-3-running-code-on-cleanup-with-the-drop-trait]]
