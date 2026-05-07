---
title: "Default Trait Implementations"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits]
source_count: 1
---

# Default Trait Implementations

## Short Definition

Default trait implementation trait methodiga body berib, implementor type'ga uni keep yoki override qilish imkonini beradi.

## Why It Matters

Default methods boilerplateni kamaytiradi. Trait implementor faqat o'ziga xos qismini yozadi, trait esa common behaviorni beradi.

## Mental Model

Trait method body bilan yozilsa, implementor uni yozmasa ham method mavjud bo'ladi. Implementor shu methodni qayta yozsa, default behavior override qilinadi. Default methodlar shu traitdagi boshqa methodsni chaqirishi mumkin.

Cheklov: overriding implementation ichidan shu methodning default implementationini chaqirib bo'lmaydi.

## Syntax and Examples

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

impl Summary for NewsArticle {}
```

Required methodga tayanadigan default:

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

## Common Mistakes

- Default method bor bo'lsa, traitni implement qilish shart emas deb o'ylash.
- Required methodni bermasdan default method undan foydalanishini unutish.
- Override qilgan method ichidan default implementationni chaqirish mumkin deb o'ylash.
- Empty `impl Trait for Type {}` block default methodsni ishlatish uchun valid ekanini bilmaslik.

## Related Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[summary-trait-example|Summary trait example]]

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
