---
title: "PartialEq"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-17
tags: [rust, traits, comparisons, equality]
source_count: 2
---

# PartialEq

## Short Definition

`PartialEq` trait'i qiymatlarni equality comparison uchun solishtirishga ruxsat beradi va `==` hamda `!=` operatorlarini yoqadi.

## Why It Matters

Custom type'lar equality semantics'ga ega bo'lishi kerak bo'lsa, `PartialEq` asosiy trait. `assert_eq!` ham ikki qiymatni solishtirish uchun shunga tayanadi.

## Mental Model

`PartialEq` "bu type uchun tenglikni mantiqan ta'riflash mumkin" degani. `==` odatda `eq`, `!=` esa `ne` semanticsiga ulanadi. "Partial" qismi shuni eslatadi: barcha typelar self-equalityni to'liq kafolatlay olmaydi, masalan float `NaN`.

## Syntax and Examples

```rust
#[derive(PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

assert_eq!(Point { x: 1, y: 2 }, Point { x: 1, y: 2 });
```

Derive bo'lganda:

- struct'lar barcha fieldlar teng bo'lsa teng;
- enum'lar bir xil variant va ichki qiymatlar teng bo'lsa teng.

Qo'lda implement qilish ham mumkin:

```rust
impl PartialEq for Point {
    fn eq(&self, other: &Self) -> bool {
        self.x == other.x && self.y == other.y
    }
}
```

## Common Mistakes

- `PartialEq` bor bo'lsa `Eq` ham avtomatik mumkin deb o'ylash.
- Equality semantics'ni domen mantiqiga mos kelmaydigan qilib derive qilish.
- `assert_eq!` faqat `PartialEq` talab qiladi deb o'ylab, `Debug` talabini unutish.
- `derive(PartialEq)` barcha fieldlar `PartialEq` bo'lmasa ham ishlaydi deb o'ylash.

## Related Concepts

- [[eq-trait]]
- [[partial-ord]]
- [[debug-trait]]
- [[derive-attribute]]
- [[attribute]]
- [[operator-overloading]]

## Sources

- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
- [[wiki/sources/22-3-c-derivable-traits|22.3]]
