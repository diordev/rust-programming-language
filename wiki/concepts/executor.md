---
title: "Executor"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, runtime]
source_count: 1
---

# Executor

## Short Definition

Executor - [[future|Future]] qiymatlarini navbatga olib, ularni `poll` qilish orqali bajaradigan async scheduling komponenti.

## Why It Matters

Rust `async fn` yozishga imkon beradi, lekin bu future'larni bajaradigan executorni standard library ichida bermaydi. Shuning uchun async kod ishlashi uchun `block_on`, `tokio`, `async-std`, yoki boshqa runtime kerak.

## Mental Model

Executor task queue bilan ishlaydigan operatorga o'xshaydi:

1. Future queue'ga keladi.
2. Executor uni `poll` qiladi.
3. `Ready` bo'lsa, task tugaydi.
4. `Pending` bo'lsa, future saqlanadi.
5. [[waker|Waker]] signal berganda future yana queue'ga qaytadi.

## Syntax and Examples

```rust
use futures::executor::block_on;

async fn value() -> i32 {
    42
}

fn main() {
    let result = block_on(value());
    println!("{result}");
}
```

Minimal executor konseptual oqimi:

```rust
match future.as_mut().poll(&mut cx) {
    Poll::Ready(()) => finish_task(),
    Poll::Pending => store_and_wait_for_wake(),
}
```

## Common Mistakes

- Executorni async runtime bilan bir xil narsa deb qabul qilish. Runtime odatda executor bilan birga timer, I/O driver, task spawning, va thread poolni ham beradi.
- `async fn` chaqirilishi bilan ish boshlanadi deb o'ylash. Future executor tomonidan `poll` qilinmaguncha ishlamaydi.
- `Pending` future'ni waker signalisiz qayta-qayta poll qilish. Bu CPU'ni bekor band qiladi.

## Related Concepts

- [[async-runtime|async runtime]]
- [[future|Future]]
- [[polling]]
- [[waker|Waker]]
- [[block-on|block_on]]
- [[async-task|async task]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

