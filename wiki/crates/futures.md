---
title: "futures"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, async, futures]
source_count: 1
---

# futures

## Short Definition

`futures` - Rust async ecosystemidagi utility crate; source'da `futures::executor::block_on` sodda executor misoli sifatida ishlatiladi.

## Why It Matters

Rust standard library [[future|Future]] traitini beradi, lekin executor bermaydi. `futures` crate'i o'quv va utility holatlarida future'lar bilan ishlash uchun qo'shimcha APIlar beradi.

## Installation

```toml
[dependencies]
futures = "0.3"
```

## Main APIs In This Wiki

- `futures::executor::block_on` - bitta future'ni joriy threadda tugaguncha bajaradi.

## Example

```rust
use futures::executor::block_on;

async fn get_1() -> i32 {
    1
}

fn main() {
    let result = block_on(get_1());
    println!("{result}");
}
```

## Common Mistakes

- `futures::executor::block_on`ni high-load backend runtime o'rnida ishlatish. Source uni sodda executor demonstratsiyasi sifatida beradi.
- `futures` crate'ini Rust standard library qismi deb o'ylash. Bu alohida crate.

## Related Concepts

- [[future|Future]]
- [[executor]]
- [[block-on|block_on]]
- [[async-runtime|async runtime]]
- [[polling]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

