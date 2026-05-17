---
title: "Supertraits"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, traits, bounds]
source_count: 2
---

# Supertraits

## Short Definition

`trait MyTrait: SuperTrait { ... }` — bitta trait'ning ishlashi uchun boshqa trait'ning ham implement qilinishi shart. **Supertrait** — boshqa trait'ga "supertrait" hisoblanadi, agar ikkinchisi birinchini talab qilsa.

## Why It Matters

Trait body ichida boshqa trait'ning method'larini ishlatish kerak bo'lsa — supertrait belgilash kerak. Misol: `OutlinePrint: Display` — `OutlinePrint::outline_print` ichida `to_string()` ishlatadi (`Display`'dan keladi).

## Mental Model

```
trait OutlinePrint: Display { ... }
                   ↑
              "for any T to impl OutlinePrint, T must impl Display"
```

Bu `T: Display + OutlinePrint` trait bound'ga juda o'xshash, lekin trait ta'rifining bir qismi. Backend traits source buni beginner tilida "trait inheritance" deb tanishtiradi, lekin amaliy ma'no shu: implementor parent traitlarni ham bajarishi shart.

## Syntax and Examples

### Asosiy

```rust
use std::fmt;

trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();   // <-- Display'dan to_string()
        let len = output.len();
        println!("{}", "*".repeat(len + 4));
        println!("*{}*", " ".repeat(len + 2));
        println!("* {output} *");
        println!("*{}*", " ".repeat(len + 2));
        println!("{}", "*".repeat(len + 4));
    }
}
```

Endi `OutlinePrint`'ni implement qilmoqchi bo'lgan har qanday tip `Display`'ni ham implement qilishi kerak.

### Implement — `Display` ni ham implementatsiya qilish

```rust
use std::fmt;

struct Point { x: i32, y: i32 }

impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}

impl OutlinePrint for Point {}    // bo'sh impl, default outline_print ishlatadi

fn main() {
    let p = Point { x: 1, y: 3 };
    p.outline_print();
    // **********
    // *        *
    // * (1, 3) *
    // *        *
    // **********
}
```

Agar `Display` implementatsiyani qoldirsak — [[e0277-trait-bound-not-satisfied|E0277]]: "the trait `std::fmt::Display` is not implemented for `Point`."

### Bir nechta supertrait

```rust
use std::fmt::{Debug, Display};

trait Loggable: Debug + Display {
    fn log(&self) {
        eprintln!("[debug] {:?}", self);
        eprintln!("[display] {}", self);
    }
}
```

`+` bilan birlashtiriladi. Implementor barcha supertrait'larni implement qilishi kerak.

## Trait Bound vs Supertrait

| Holat | Yozish |
|-------|--------|
| Funksiya argument talabi | `fn f<T: Display>(x: T) { ... }` |
| Trait body Display'ni ishlatadi | `trait OutlinePrint: Display { ... }` |

Supertrait — **trait ta'rifining qismi**; trait bound — funksiya yoki impl block'ning qismi.

## Common Mistakes

- **Supertrait belgilamasdan trait body'da `to_string()`/`format!` ishlatish.** "Method `to_string` not found" xatosi. `Display`'ni supertrait qiling.
- **`impl Trait for Type` yozib supertrait'ni implementatsiyalashni unutish.** [[e0277-trait-bound-not-satisfied|E0277]] — supertrait talabi bajarilmagan.
- **Supertrait'ni metod bo'lish o'rniga trait bound deb hisoblash.** Trait'ning *ta'rifi* shart qiladi, function bound emas.

## Related Concepts

- [[traits]]
- [[trait-bounds|trait bounds]]
- [[trait-definitions|trait definitions]]
- [[debug-trait|Debug trait]]
- [[display-formatting|Display formatting]]
- [[default-trait-implementations|default trait implementations]]
- [[fully-qualified-syntax|fully qualified syntax]]

## Sources

- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[wiki/sources/rust-for-backend-developers-traits]]
