---
title: "OutlinePrint — supertrait misoli"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, supertraits, display]
source_count: 1
---

# OutlinePrint — Supertrait Misoli

## Listing 20-23 — Supertrait bilan trait

```rust
use std::fmt;

trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();   // Display'dan keladigan metod
        let len = output.len();
        println!("{}", "*".repeat(len + 4));
        println!("*{}*", " ".repeat(len + 2));
        println!("* {output} *");
        println!("*{}*", " ".repeat(len + 2));
        println!("{}", "*".repeat(len + 4));
    }
}
```

`trait OutlinePrint: fmt::Display` — `OutlinePrint` ni implement qilmoqchi bo'lgan har qanday tip `Display` ni ham implement qilishi shart. Trait body'da `to_string()` ishlatiladi (u faqat `Display` tiplari uchun mavjud).

## Implement Qilish — `Display` ham kerak

```rust
use std::fmt;

struct Point { x: i32, y: i32 }

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

impl OutlinePrint for Point {}    // bo'sh — default outline_print ishlaydi

fn main() {
    let p = Point { x: 1, y: 3 };
    p.outline_print();
}
```

Output:

```text
**********
*        *
* (1, 3) *
*        *
**********
```

## Display'siz — Compilation Xato

```rust
struct Point { x: i32, y: i32 }
// Display impl yo'q

impl OutlinePrint for Point {}    // E0277
```

```text
error[E0277]: `Point` doesn't implement `std::fmt::Display`
   |
20 | impl OutlinePrint for Point {}
   |                       ^^^^^ the trait `std::fmt::Display`
   |                             is not implemented for `Point`
   |
note: required by a bound in `OutlinePrint`
```

Yechim: `Display` ni ham implement qilish.

## Tahlil

- **Supertrait** — trait ta'rifining qismi (`: fmt::Display`).
- **Trait body'da supertrait method'lari ishlatilishi mumkin** — `self.to_string()`, `format!("{self}")`.
- **Implementor majburiyat** — supertrait'ni alohida implementatsiya qilish kerak.
- **Bo'sh `impl OutlinePrint for Point {}`** — chunki `outline_print` default implementatsiyaga ega.

## Bir Nechta Supertrait

```rust
trait Loggable: std::fmt::Debug + std::fmt::Display {
    fn log(&self) {
        eprintln!("[debug] {:?}", self);
        eprintln!("[display] {}", self);
    }
}
```

`+` bilan birlashtiriladi. Implementor `Debug` va `Display` ikkalasini ham implementatsiya qilishi kerak.

## Related

- [[supertraits]]
- [[traits]]
- [[trait-bounds|trait bounds]]
- [[display-formatting|Display formatting]]
- [[default-trait-implementations|default trait implementations]]
- [[e0277-trait-bound-not-satisfied|E0277]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
