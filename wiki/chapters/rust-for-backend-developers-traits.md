---
title: "Rust for Backend Developers: Traits"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, traits, chapter]
source_count: 1
---

# Rust for Backend Developers: Traits

## Learning Goal

Traits orqali behavior abstraction, static/dynamic dispatch, va trait object cheklovlarini bir beginner-friendly modelga yig'ish.

## Main Ideas

- Trait type bilan interaction contractini belgilaydi.
- `impl Trait` static dispatch va monomorphizationga olib keladi.
- `dyn Trait` trait object orqali dynamic dispatch beradi.
- `impl Trait` return position bitta concrete type bilan ishlaydi; branchlar turlicha bo'lsa `Box<dyn Trait>` kerak.
- Default impl, supertraits, orphan rule, `Self`, va `unsafe trait` trait ecosystemning muhim qoidalari.

## Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[impl-trait|impl Trait]]
- [[static-dispatch|static dispatch]]
- [[trait-object|trait object]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[polymorphism]]
- [[monomorphization]]
- [[orphan-rule|orphan rule]]
- [[default-trait-implementations|default trait implementations]]
- [[supertraits]]
- [[trait-bounds|trait bounds]]
- [[self-type|Self type]]
- [[box-t|Box<T>]]
- [[sized-trait|Sized trait]]
- [[dyn-compatibility|dyn compatibility]]
- [[unsafe-trait|unsafe trait]]

## Examples

```rust
trait CanIntroduce {
    fn introduce(&self) -> String;
}

fn print_introduction(v: &impl CanIntroduce) {
    println!("{}", v.introduce());
}
```

```rust
fn print_dyn(v: &dyn CanIntroduce) {
    println!("{}", v.introduce());
}
```

## Exercises

- `trait HasId` yozib, ikki xil struct uchun implement qiling.
- `make_actor(kind: bool) -> Box<dyn Trait>` ko'rinishida factory function yozing.

## Review Questions

- `impl Trait` va `dyn Trait` orasidagi asosiy runtime farq nima?
- Nega `dyn Trait`ni odatda `&dyn Trait` yoki `Box<dyn Trait>` orqali uzatamiz?
- Orphan rule qaysi collisionni oldini oladi?
- `unsafe trait` nega method ichida albatta `unsafe` bo'lishini anglatmaydi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[traits]]
- [[trait-object|trait object]]
- [[unsafe-trait|unsafe trait]]
