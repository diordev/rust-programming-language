---
title: "async closure and block"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, closures, example]
source_count: 1
---

# async closure and block

## Context

Future faqat `async fn` chaqiruvidan yaratilmaydi. `async {}` block darhol future yaratadi, `async || {}` closure esa chaqirilganda future yaratadi.

## Code

```rust
use futures::executor::block_on;

fn main() {
    let from_block = async { 1 };
    let result = block_on(from_block);
    println!("{result}");

    let closure = async || { 2 };
    let from_closure = closure();
    let result = block_on(from_closure);
    println!("{result}");
}
```

## Explanation

- `async { 1 }` expression future yaratadi.
- `async || { 2 }` callable closure yaratadi.
- `closure()` chaqirilganda future yaratiladi.
- Ikkala future ham executor orqali bajariladi.

## Related Concepts

- [[async-block|async block]]
- [[async-closure|async closure]]
- [[future|Future]]
- [[block-on|block_on]]
- [[closures]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

