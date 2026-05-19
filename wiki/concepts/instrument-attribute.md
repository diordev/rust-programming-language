---
title: "#[instrument]"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tracing, attribute, logging]
source_count: 1
---

# #[instrument]

## Short Definition

`tracing` attribute macro bo'lib, function body'ni span ichida bajaradi va argumentlarni span fields sifatida qo'shadi.

## Why It Matters

Backend handler/service functionlarda contextni qo'lda span yaratmasdan bir xil formatda qo'shish mumkin.

## Mental Model

Function chaqiruvi logical operation spaniga aylanadi.

## Syntax and Examples

```rust
#[tracing::instrument(skip(password), fields(user_id = %user_id), ret)]
fn login(user_id: u64, password: &str) -> bool {
    true
}
```

## Common Mistakes

- Sensitive argumentlarni avtomatik log field sifatida chiqarish.
- Return value katta yoki sensitive bo'lsa `ret`ni ehtiyotsiz yoqish.

## Related Concepts

- [[tracing-span|tracing span]]
- [[structured-logging|structured logging]]
- [[attribute-like-macros|attribute-like macros]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

