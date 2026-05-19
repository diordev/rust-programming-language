---
title: "Async Closure"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, closures]
source_count: 1
---

# Async Closure

## Short Definition

Async closure - `async || { ... }` sintaksisi bilan yoziladigan closure; chaqirilganda [[future|Future]] qaytaradi.

## Why It Matters

Async closure callback yoki higher-order API ichida async ishni kechiktirib yaratish kerak bo'lganda foydali. U oddiy closure kabi capture qiladi, lekin body natijasi future orqali olinadi.

## Mental Model

Oddiy closure `|| 1` chaqirilganda `1` qaytaradi. Async closure `async || 1` chaqirilganda `1` emas, `Future<Output = i32>` qaytaradi.

## Syntax and Examples

```rust
let closure = async || { 1 };
let future = closure();
let result = futures::executor::block_on(future);
assert_eq!(result, 1);
```

Capture bilan:

```rust
let base = 41;
let closure = async || base + 1;
let result = futures::executor::block_on(closure());
assert_eq!(result, 42);
```

## Common Mistakes

- Closure e'lon qilinganda async ish boshlanadi deb o'ylash. Ish closure chaqirilib future yaratilgandan keyin ham executor `poll` qilmaguncha yurmaydi.
- Async closure va [[async-block|async block]]ni aralashtirish. Async block darhol future yaratadi; async closure future yaratadigan callable qiymat yaratadi.
- MSRVni e'tiborsiz qoldirish. Async closure zamonaviy Rustda mavjud, eski toolchainlarda ishlamasligi mumkin.

## Related Concepts

- [[async-block|async block]]
- [[closures]]
- [[async-await|async/await]]
- [[future|Future]]
- [[block-on|block_on]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

