---
title: "cargo fix unused mut example"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, cargo-fix, warnings]
source_count: 1
---

# cargo fix unused mut example

Compiler `unused_mut` warningini automatic suggestion bilan tuzatish mumkin.

```rust
fn main() {
    let mut x = 42;
    println!("{x}");
}
```

`cargo fix`dan keyin:

```rust
fn main() {
    let x = 42;
    println!("{x}");
}
```

## Why It Matters

Bu `cargo fix` / `rustfix` mechanical warning fix'larni tezlashtirishini ko'rsatadi.

## Related

- [[cargo-fix]]
- [[compiler]]
