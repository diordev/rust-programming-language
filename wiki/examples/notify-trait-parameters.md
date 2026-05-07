---
title: "notify Trait Parameters"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, traits, generics]
source_count: 1
---

# notify Trait Parameters

## Summary

`notify` functioni traitni parameter contracti sifatida ishlatadi. `&impl Summary` oddiy va concise; `T: Summary` esa same-type constraints kabi murakkab holatlarni ifodalaydi.

## Code

`impl Trait` parameter:

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

Equivalent trait bound:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Two possibly different concrete types:

```rust
pub fn notify(item1: &impl Summary, item2: &impl Summary) {
    println!("{} {}", item1.summarize(), item2.summarize());
}
```

Two values of the same concrete type:

```rust
pub fn notify<T: Summary>(item1: &T, item2: &T) {
    println!("{} {}", item1.summarize(), item2.summarize());
}
```

Multiple bounds:

```rust
pub fn notify<T: Summary + Display>(item: &T) {
    println!("Breaking news! {}", item.summarize());
    println!("{}", item);
}
```

## Explanation

- `impl Trait` simple parameter cases uchun concise.
- `T: Trait` type relationshipni ifodalaydi, masalan ikki parameter bir xil concrete type bo'lishi.
- `Summary + Display` bodyga ikkala capabilityni beradi.
- Bounds ko'paysa, [[where-clauses|where clause]] o'qilishni yaxshilaydi.

## Related Concepts

- [[impl-trait|impl Trait]]
- [[trait-bounds|trait bounds]]
- [[where-clauses|where clauses]]
- [[traits]]
- [[generic-functions|generic functions]]
- [[display-formatting|Display]]

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
