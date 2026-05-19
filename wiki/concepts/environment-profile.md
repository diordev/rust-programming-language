---
title: "Environment Profile"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, configuration, environment-variables]
source_count: 1
---

# Environment Profile

## Short Definition

Application qaysi environment config layerini yuklashini belgilaydigan profile, masalan `dev`, `stg`, `prod`.

## Why It Matters

Local, staging, va production bir xil binary bilan, lekin boshqa configuration bilan ishlaydi.

## Mental Model

`PROFILE=prod` -> `config/prod.toml` layer yuklanadi.

## Syntax and Examples

```rust
let profile = std::env::var("PROFILE").unwrap_or_else(|_| "dev".into());
```

## Common Mistakes

- Productionda missing `PROFILE` uchun `dev`ga silently fallback qilish.
- Unknown profile file missing bo'lsa errorni yashirish.

## Related Concepts

- [[env-var|env::var]]
- [[configuration-layering|configuration layering]]
- [[application-configuration|application configuration]]

## Sources

- [[wiki/sources/rust-for-backend-developers-application-configuration]]

