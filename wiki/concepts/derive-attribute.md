---
title: "derive Attribute"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, attributes, traits]
source_count: 4
---

# derive Attribute

## Short Definition

`#[derive(...)]` compilerga type uchun ayrim standard trait implementationsni avtomatik yaratishni so'raydigan [[attribute]].

## Why It Matters

`#[derive(Debug)]` custom structni [[debug-formatting|Debug formatting]] bilan chiqarishga ruxsat beradi. `PartialEq`, `Clone`, va `Copy` kabi traitlar uchun esa qo'lda yoziladigan mechanical impl'ni qisqartiradi.

## Mental Model

Derive "bu traitning default mechanical implementationini men uchun generate qil" degan signal. Bu explicit opt in, Rust custom type behaviorini taxmin qilmaydi.

Compiler derive qilinadigan trait uchun type fieldlarini ko'radi va agar barcha zarur fieldlar ham mos trait'ga ega bo'lsa impl generatsiya qiladi. Masalan `#[derive(Clone)]` field-by-field `clone()` qiladi; `#[derive(Copy)]` esa type'ni implicit copy semanticsga opt in qiladi va `Clone`ni ham talab qiladi.

Standard derives compiler/standard tooling tomonidan beriladi. [[custom-derive-macros|Custom derive macros]] esa crate authors tomonidan procedural macro sifatida yaratiladi, masalan `#[derive(HelloMacro)]`.

## Syntax and Examples

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
```

Bir nechta traitni birga derive qilish:

```rust
#[derive(Debug, PartialEq, Clone, Copy)]
struct Point2D {
    x: i32,
    y: i32,
}
```

Custom derive:

```rust
#[derive(HelloMacro)]
struct Pancakes;
```

### Standard derivable traits

Appendix C bo'yicha standard library'dan mechanical derive qilinadigan asosiy traitlar:

- [[debug-trait|Debug]]
- [[partial-eq|PartialEq]]
- [[eq-trait|Eq]]
- [[partial-ord|PartialOrd]]
- [[ord-trait|Ord]]
- [[clone]]
- [[copy-trait|Copy]]
- [[hash-trait|Hash]]
- [[default-trait|Default]]

### Nega `Display` derive qilinmaydi?

`Display` user-facing formatting traiti. Compiler sizning type'ingizni oxirgi foydalanuvchi uchun qaysi formatda ko'rsatish kerakligini bilolmaydi, shu sabab uni qo'lda implement qilish kerak.

## Common Mistakes

- Derive hamma traitlar uchun mavjud deb o'ylash.
- Derived behavior custom behavior bilan bir xil bo'ladi deb kutish.
- Custom derive macro uchun alohida `proc-macro` crate kerakligini unutish.
- `Display` yoki boshqa user-facing semantic traitlar ham mechanical derive bo'ladi deb o'ylash.
- `Copy` derive qilinsa `Clone` talabi yo'q deb o'ylash.
- `derive` attribute oilasidan alohida maxsus sintaksis deb ko'rish.

## Related Concepts

- [[debug-trait|Debug trait]]
- [[attribute]]
- [[partial-eq|PartialEq]]
- [[eq-trait|Eq]]
- [[ord-trait|Ord]]
- [[hash-trait|Hash]]
- [[default-trait|Default]]
- [[marker-trait|marker trait]]
- [[custom-derive-macros|custom derive macros]]
- [[procedural-macros|procedural macros]]
- [[traits]]
- [[structs]]

## Sources

- [[5-2-an-example-program-using-structs]]
- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
- [[wiki/sources/20-5-macros|20.5 Macros]]
- [[wiki/sources/22-3-c-derivable-traits|22.3]]
