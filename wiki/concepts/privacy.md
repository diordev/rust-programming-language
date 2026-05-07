---
title: "Privacy"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules, api-design]
source_count: 4
---

# Privacy

## Short Definition

Privacy Rustda module ichidagi item'lar tashqi code uchun ko'rinadimi yoki private implementation detail bo'lib qoladimi, shuni belgilaydigan qoida.

## Why It Matters

Rust item'lari private by default. Bu crate ichida implementation detail'larni yashirish va public API'ni ongli ravishda tanlash imkonini beradi. Public API qanchalik aniq bo'lsa, boshqa code unga shunchalik barqaror depend qiladi.

## Mental Model

Module ichidagi code ichkaridan tartiblangan xona kabi: parent yoki external code hamma narsani avtomatik ko'rmaydi. Biror module yoki item tashqaridan ishlatilishi kerak bo'lsa, u [[pub-keyword|pub]] bilan public qilinadi.

Parent module child module ichidagi private item'larni ishlata olmaydi. Child module esa ancestor modulesdagi item'larni ko'ra oladi. Bu child modules implementation detail'larini yashirishga yordam beradi.

[[use-declarations|Use declarations]] privacy'ni chetlab o'tmaydi: path shortcut sifatida scope'ga olib kirilsa ham, item public bo'lishi yoki access qoidalari ruxsat berishi kerak.

## Syntax and Examples

```rust
pub mod garden;
```

Bu `garden` module'ini public qiladi.

```rust
#[derive(Debug)]
pub struct Asparagus {}
```

Bu `Asparagus` struct'ini public qiladi. Public module ichidagi item public bo'lishi uchun item'ning o'zi ham `pub` bo'lishi kerak.

Private module yoki function'ni path orqali chaqirish [[e0603-private-item|E0603 private item]]ga olib keladi.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}
```

Bu yerda `hosting` module ham, `add_to_waitlist` function ham public.

## Common Mistakes

- Public module ichidagi barcha item'lar avtomatik public deb o'ylash.
- Private item'larni public API sifatida external code ishlata oladi deb kutish.
- Privacy faqat security uchun, code organization uchun emas deb o'ylash.
- Path to'g'ri bo'lsa privacy irrelevant deb o'ylash.
- `use` private item'ni tashqaridan ishlatishga ruxsat beradi deb o'ylash.

## Related Concepts

- [[module]]
- [[module-system|module system]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[scope]]
- [[paths]]
- [[api-design|API design]]
- [[e0603-private-item|E0603 private item]]
- [[use-declarations|use declarations]]

## Sources

- [[7-packages-crates-and-modules-the-rust-programming-language]]
- [[7-2-control-scope-and-privacy-with-modules-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
