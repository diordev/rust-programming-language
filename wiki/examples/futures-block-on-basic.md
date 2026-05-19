---
title: "futures block_on basic"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, futures, example]
source_count: 1
---

# futures block_on basic

## Context

`async fn` chaqiruvi natijani emas, [[future|Future]] qaytaradi. `futures::executor::block_on` shu future'ni joriy threadda bajarib, natijasini qaytaradi.

## Code

```rust
use futures::executor::block_on;

async fn get_1() -> i32 {
    1
}

fn main() {
    let future = get_1();
    let result = block_on(future);
    println!("{result}");
}
```

`Cargo.toml`:

```toml
[dependencies]
futures = "0.3"
```

## Explanation

- `get_1()` `i32` qaytarmaydi, `impl Future<Output = i32>` yaratadi.
- `block_on` future'ni executor orqali `poll` qiladi.
- Future `Ready(1)` bo'lganda `block_on` `1`ni qaytaradi.

## Related Concepts

- [[wiki/crates/futures|futures]]
- [[block-on|block_on]]
- [[future|Future]]
- [[executor]]
- [[async-await|async/await]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

