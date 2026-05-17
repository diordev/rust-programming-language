---
title: "GUI Draw Trait"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, trait-objects, example]
source_count: 1
---

# GUI Draw Trait

## Description

Kengaytiriladigan GUI kutubxonasi: `Draw` trait va `Box<dyn Draw>` bilan heterojen komponentlar to'plami. Foydalanuvchi kutubxonadan tashqarida yangi tiplar qo'sha oladi.

## Source

[[18-2-using-trait-objects-to-abstract-over-shared-behavior]] — Listing 18-3 dan 18-9 gacha.

## Kutubxona (gui crate)

```rust
// src/lib.rs
pub trait Draw {
    fn draw(&self);
}

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}

impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

pub struct Button {
    pub width: u32,
    pub height: u32,
    pub label: String,
}

impl Draw for Button {
    fn draw(&self) {
        println!("Drawing Button: {} ({}x{})", self.label, self.width, self.height);
    }
}
```

## Foydalanuvchi kodi (tashqi crate)

```rust
use gui::{Button, Draw, Screen};

// Kutubxona BILMAGAN yangi tip:
struct SelectBox {
    width: u32,
    height: u32,
    options: Vec<String>,
}

impl Draw for SelectBox {
    fn draw(&self) {
        println!("Drawing SelectBox: {:?}", self.options);
    }
}

fn main() {
    let screen = Screen {
        components: vec![
            Box::new(SelectBox {
                width: 75,
                height: 10,
                options: vec![
                    String::from("Yes"),
                    String::from("Maybe"),
                    String::from("No"),
                ],
            }),
            Box::new(Button {
                width: 50,
                height: 10,
                label: String::from("OK"),
            }),
        ],
    };

    screen.run();
    // Drawing SelectBox: ["Yes", "Maybe", "No"]
    // Drawing Button: OK (50x10)
}
```

## Xato: Draw implement qilmagan tur

```rust
components: vec![Box::new(String::from("Hi"))]
// error[E0277]: the trait `Draw` is not implemented for `String`
```

## Generic versiya bilan solishtirish

```rust
// Generic (homojen — faqat bitta T):
pub struct Screen<T: Draw> { pub components: Vec<T> }
// Screen<Button> yoki Screen<SelectBox> — aralash emas

// Trait object (heterojen — aralash turlar):
pub struct Screen { pub components: Vec<Box<dyn Draw>> }
// Button va SelectBox birgalikda ✓
```

## Concepts Used

- [[trait-object|trait object]] — `Box<dyn Draw>`
- [[dynamic-dispatch|dynamic dispatch]] — `run()` da vtable orqali to'g'ri `draw` topiladi
- [[polymorphism]] — heterojen to'plam
- [[traits]] — `Draw` trait ta'rifi

## Related Pages

- [[18-2-using-trait-objects]]
- [[spreadsheet-cell-vector]] — enum bilan heterojen to'plam (8-bob alternativasi)
