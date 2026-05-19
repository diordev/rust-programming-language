---
title: "Logging"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging, backend]
source_count: 1
---

# Logging

## Short Definition

Application runtime’da sodir bo‘layotgan eventlarni yozib borish.

## Why It Matters

Backendda debugging, incident analysis, audit, va observability uchun loglar asosiy signal.

## Mental Model

Log event - timestamp, level, target, message, va optional structured fieldsdan iborat runtime signal.

## Syntax and Examples

```rust
tracing::info!("user created");
```

## Common Mistakes

- `println!`ni production logging o'rnida ishlatish.
- Sensitive data'ni log field sifatida chiqarish.

## Related Concepts

- [[structured-logging|structured logging]]
- [[log-levels|log levels]]
- [[tracing-span|tracing span]]
- [[wiki/crates/tracing]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

