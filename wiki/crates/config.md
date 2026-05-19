---
title: "config"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, configuration]
source_count: 1
---

# config

## Short Definition

`config` file va environment kabi sourcelardan application configuration yig'ib, serde orqali typed structga deserialize qiladigan crate.

## Cargo Dependency

```toml
[dependencies]
config = "0.15"
serde = { version = "1", features = ["derive"] }
```

## Basic Usage

```rust
let cfg = config::Config::builder()
    .add_source(config::File::with_name("config/default.toml"))
    .build()?;
```

## Notes

- Source snapshot `0.15`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- Source order precedence beradi: keyin qo'shilgan source oldingi qiymatlarni override qilishi mumkin.
- `Environment::with_prefix(...).separator("__")` nested valuesni env variable orqali override qiladi.

## Related Pages

- [[application-configuration|application configuration]]
- [[configuration-layering|configuration layering]]
- [[config-deserialization|config deserialization]]
- [[wiki/crates/serde]]
- [[wiki/sources/rust-for-backend-developers-application-configuration]]

