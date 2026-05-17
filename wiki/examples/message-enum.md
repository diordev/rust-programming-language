---
title: "Message Enum Example"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, enums]
source_count: 1
---

# Message Enum Example

## Goal

Listing 6-2 misoli — `Message` enum to'rt xil variant shaklini bitta type ostida birlashtiradi va `impl` orqali method qo'shish mumkinligini ko'rsatadi.

## Code

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

impl Message {
    fn call(&self) {
        // method body would be defined here
    }
}

fn main() {
    let m = Message::Write(String::from("hello"));
    m.call();
}
```

## Variant Shakllari

- `Quit` — datasiz, *unit-like*. Hech qanday payload tashimaydi.
- `Move { x: i32, y: i32 }` — *named fields*, struct'ga o'xshash.
- `Write(String)` — bitta `String`'ni o'rab oluvchi *tuple-like*.
- `ChangeColor(i32, i32, i32)` — uchta `i32`'li tuple-like.

To'rt variant alohida structlar bilan ham yozilishi mumkin (`QuitMessage`, `MoveMessage`, `WriteMessage`, `ChangeColorMessage`), lekin "har qanday Message qabul qiluvchi" funksiya yozish o'shanda qiyin bo'ladi. Enum bilan ular hammasi bitta `Message` type ostida.

## Method via `impl`

`impl` block enum'ga ham xuddi struct'ga qo'shilgandek method beradi:

```rust
impl Message {
    fn call(&self) {
        // ...
    }
}
```

`&self` enum instance'ini borrow qiladi. Method body ichida `match self { ... }` yozib variantga qarab har xil branch ishlatish mumkin (Chapter 6.2'da batafsil).

## Variant Constructorlari

Har bir variant nomi constructor sifatida ham ishlaydi:

- `Message::Write(String::from("hi"))` — `Write` variantini yasaydi.
- `Message::ChangeColor(255, 0, 0)` — uch `i32`dan variant yasaydi.
- `Message::Move { x: 10, y: 20 }` — named-field variant yaratish (struct literal syntax).
- `Message::Quit` — datasiz variant, prefiks bilan to'g'ridan-to'g'ri value.

## Related Concepts

- [[enums]]
- [[enum-variants|enum variants]]
- [[methods]]
- [[impl-block|impl block]]
- [[constructor-functions|constructor functions]]

## Sources

- [[6-1-defining-an-enum]]
