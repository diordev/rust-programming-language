---
title: "Root Cause"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, error-handling, diagnostics]
source_count: 1
---

# Root Cause

## Short Definition

Error chain'dagi eng ichki, asl sabab bo'lgan error.

## Why It Matters

Wrapping va context ko'payganda asl failure signalini yo'qotmaslik kerak.

## Mental Model

Tashqi error "qayerda yiqildi", root cause esa "nima yiqildi".

## Syntax and Examples

```rust
eprintln!("It failed with: {}", e.root_cause());
```

## Common Mistakes

- Context bilan root cause'ni bir xil narsa deb o'ylash.

## Related Concepts

- [[error-context]]
- [[error-wrapping]]
- [[wiki/crates/anyhow]]

## Sources

- [[wiki/sources/rust-for-backend-developers-error-handling]]
