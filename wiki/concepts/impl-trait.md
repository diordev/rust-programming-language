---
title: "impl Trait"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits, generics]
source_count: 1
---

# impl Trait

## Short Definition

`impl Trait` concrete type nomini yozmasdan, biror traitni implement qilgan type'ni parameter yoki return type sifatida ifodalash syntax'i.

## Why It Matters

`impl Trait` oddiy trait-bound holatlarni ixcham yozadi. Parameterda "shu traitni implement qilgan har qanday type", return positionda esa "shu traitni implement qiladigan bitta concrete type" degan contract beradi.

## Mental Model

Parameter position:

```rust
pub fn notify(item: &impl Summary)
```

Bu `Summary` implement qilgan har qanday type'ni qabul qiladi. Ikki parameter alohida `impl Summary` bo'lsa, ular turli concrete type bo'lishi mumkin.

Return position:

```rust
fn returns_summarizable() -> impl Summary
```

Bu caller concrete type nomini bilmasdan `Summary` behavioridan foydalanishini bildiradi. Lekin function body bitta concrete return type qaytarishi kerak.

## Syntax and Examples

Parameter:

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

Multiple traits:

```rust
pub fn notify(item: &(impl Summary + Display)) {
    println!("Breaking news! {}", item.summarize());
    println!("{}", item);
}
```

Return:

```rust
fn returns_summarizable() -> impl Summary {
    SocialPost {
        username: String::from("horse_ebooks"),
        content: String::from("of course, as you probably already know, people"),
        reply: false,
        repost: false,
    }
}
```

## Common Mistakes

- Return position `impl Trait` har branchda boshqa concrete type qaytarishi mumkin deb o'ylash.
- Ikkita `&impl Summary` parameter bir xil concrete type bo'lishi kerak deb o'ylash.
- Same-type constraint kerak bo'lsa, [[trait-bounds|trait bound]] syntax ishlatish kerakligini unutish.
- `impl Trait`ni trait object bilan bir xil deb tushunish.

## Related Concepts

- [[traits]]
- [[trait-bounds|trait bounds]]
- [[generic-functions|generic functions]]
- [[where-clauses|where clauses]]
- [[summary-trait-example|Summary trait example]]
- [[notify-trait-parameters|notify trait parameters]]

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
