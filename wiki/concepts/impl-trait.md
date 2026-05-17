---
title: "impl Trait"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-16
tags: [rust, traits, generics]
source_count: 4
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

Backend beginner source bu limitni juda aniq ko'rsatadi: `if`ning bir branchi `Person`, ikkinchisi `Dog` qaytarsa, `-> impl CanIntroduce` ishlamaydi. Shunday holatda ko'pincha `Box<dyn Trait>`ga o'tiladi.

Return position `impl Trait` [[opaque-types|opaque type]] yaratadi: concrete type compilerga ma'lum, callerga yashirin. Har bir `impl Trait` return joyi alohida opaque type bo'ladi.

Backend beginner source yana bitta foydali chegarani ko'rsatadi: `impl Trait` nested trait bound ichidagi return type'ni ifodalay olmaydi. Masalan `impl FnMut() -> impl Display` yozuvi rad etiladi; bunday holatda outer function generic parametrlari bilan `F: FnMut() -> R, R: Display` shakliga o'tish kerak.

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

Not allowed inside `Fn` trait bound:

```rust
fn print_produced(mut f: impl FnMut() -> impl Display) {
    println!("{}", f());
}
```

Instead:

```rust
fn print_produced<F, R>(mut f: F)
where
    F: FnMut() -> R,
    R: Display,
{
    println!("{}", f());
}
```

## Common Mistakes

- Return position `impl Trait` har branchda boshqa concrete type qaytarishi mumkin deb o'ylash.
- Ikki alohida functiondagi `impl Fn(...)` return typelari bir xil bo'ladi deb o'ylash.
- Ikkita `&impl Summary` parameter bir xil concrete type bo'lishi kerak deb o'ylash.
- Same-type constraint kerak bo'lsa, [[trait-bounds|trait bound]] syntax ishlatish kerakligini unutish.
- `impl Trait`ni trait object bilan bir xil deb tushunish.
- Return position `impl Trait` muammosini branchlarni box qilib yechish mumkinligini bilmaslik.
- `impl Trait`ni `Fn` trait bounds ichidagi return type'ga qo'yish mumkin deb o'ylash.

## Related Concepts

- [[traits]]
- [[trait-bounds|trait bounds]]
- [[opaque-types|opaque types]]
- [[returning-closures|returning closures]]
- [[generic-functions|generic functions]]
- [[where-clauses|where clauses]]
- [[summary-trait-example|Summary trait example]]
- [[notify-trait-parameters|notify trait parameters]]
- [[box-t|Box<T>]]
- [[trait-object|trait object]]
- [[fn-traits|Fn traits]]

## Sources

- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/20-4-advanced-functions-and-closures]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-generics]]
