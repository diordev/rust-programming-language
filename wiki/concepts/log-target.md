---
title: "Log Target"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging, tracing]
source_count: 1
---

# Log Target

## Short Definition

Log event manbai sifatida ko'rsatiladigan target, odatda crate/module path.

## Why It Matters

Target bo'yicha filterlash dependency va application loglarini ajratadi.

## Mental Model

`myapp::db` targeti DB module loglarini alohida ko'rish yoki yashirishga imkon beradi.

## Syntax and Examples

```shell
$ RUST_LOG="myapp::db=debug,info" cargo run
```

## Common Mistakes

- Targetni human-readable category deb emas, module/crate path sifatida tushunmaslik.
- `with_target(false)` qilinganda outputdan target ko'rinmay qolishini unutish.

## Related Concepts

- [[env-filter|EnvFilter]]
- [[logging]]
- [[module-system|module system]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

