---
title: "Rust for Backend Developers: Application Configuration"
type: chapter
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, configuration, config, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Application Configuration

## Learning Goal

Backend app configurationni file, environment profile, va environment override layerlari orqali typed Rust structlarga yuklash.

## Main Ideas

- [[wiki/crates/config|config]] crate text config sourcelarni yig'ib, serde orqali typed structga deserialize qiladi.
- TOML Rust ecosystemida qulay default, lekin source config crate boshqa formatlarni ham qo'llashini eslatadi.
- `default.toml` common defaults beradi; `dev.toml`, `prod.toml` kabi profile files faqat farqli qiymatlarni override qiladi.
- `PROFILE` environment variable qaysi profile file yuklanishini tanlaydi.
- `Environment::with_prefix("app").separator("__")` nested configni env variable bilan override qiladi.
- Production secrets plain tracked config file’da saqlanmasligi kerak; source misolidagi passwordlar o'quv misoli sifatida qaraladi.

## Concepts

- [[application-configuration|application configuration]]
- [[config-file|config file]]
- [[toml-config|TOML config]]
- [[configuration-layering|configuration layering]]
- [[environment-profile|environment profile]]
- [[environment-overrides|environment overrides]]
- [[config-deserialization|config deserialization]]
- [[env-var|env::var]]
- [[deserialization]]

## Examples

```rust
let cfg = config::Config::builder()
    .add_source(config::File::with_name("config/default.toml"))
    .add_source(config::File::with_name(&format!("config/{profile}.toml")))
    .add_source(config::Environment::with_prefix("app").separator("__"))
    .build()?;
```

```rust
#[derive(Debug, serde::Deserialize, Clone)]
struct AppConfig {
    db: DbConfig,
    server: ServerConfig,
}
```

## Exercises

- `default.toml` va `prod.toml` bilan port override qiling.
- Missing `PROFILE` uchun default profile policy yozing.
- Env override bilan `db.host` qiymatini almashtiring.
- Config load errorsni `unwrap()`siz startup error sifatida qaytaring.

## Review Questions

- Config layeringda source order nima uchun muhim?
- File config va env override orasidagi precedence qanday?
- `serde` config deserializationga nima beradi?
- Secretlarni config file’da saqlash qaysi xavfni tug'diradi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-application-configuration]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/crates/config]]
- [[wiki/crates/serde]]

