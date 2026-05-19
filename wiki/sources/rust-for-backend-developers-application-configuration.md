---
title: "Конфигурация приложения - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, source, configuration, config]
source_count: 1
---

# Конфигурация приложения - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/58. Конфигурация приложения.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/config.html

## Detailed Summary

Bu source backend application configuration uchun [[wiki/crates/config|config]] crate'ni ko'rsatadi. Source `dotenv` va `dotenvy` haqida "abandoned" claim beradi; wiki buni source claim sifatida saqlaydi, latest ecosystem status deb da'vo qilmaydi. Asosiy pattern: config file + [[wiki/crates/serde|serde]] deserialization + environment-specific overrides.

Birinchi bo'lim `config = "0.15"` va `serde = { version = "1", features = ["derive"] }` dependency bilan boshlanadi. Example database connection settings va server listen portni `config/application.toml` fayliga chiqaradi. `Config::builder().add_source(File::with_name(...)).build()` config source'ni o'qiydi, `try_deserialize()` esa typed `AppConfig`ga aylantiradi.

Config typed structlar bilan ishlashi muhim: `AppConfig`, `DbConfig`, `ServerConfig` `Deserialize` derive qiladi. Bu `serde` bilan bir xil naming, required fields, type mismatch, va nested object modeliga tayanadi. Config file runtime boundary bo'lsa ham, application ichida typed config bilan ishlash kerak.

Multilayer config bo'limi backend environmentlar uchun muhim. Source `default.toml` common defaults, `dev.toml` va `prod.toml` environment-specific override layer sifatida ishlatilishini ko'rsatadi. `Config::builder()`ga oldin default source, keyin profile-specific source qo'shiladi; keyingi source bir xil keylarni overwrite qiladi.

Profile tanlovi `PROFILE` environment variable orqali beriladi; variable yo'q bo'lsa source default sifatida `dev` ishlatadi. Bu local ergonomika uchun qulay, lekin productionda unknown profile yoki missing file error handling aniq bo'lishi kerak.

Oxirgi bo'lim environment overrides haqida. `config::Environment::with_prefix("app").separator("__")` nested fieldlarni env variable bilan override qilishga imkon beradi. Source `APP__db__host` misolini beradi; amaliy convention sifatida uppercase `APP__DB__HOST` ko'proq uchraydi, lekin source pattern separator modelini tushuntirish uchun yetarli.

Security caveat: source example passwordni TOML file ichida ko'rsatadi. Wiki production uchun secretsni plain repository config file'da saqlashni default sifatida tavsiya qilmaydi; env override yoki secret manager boundary alohida ko'rilishi kerak.

## Key Concepts

- [[application-configuration|application configuration]]
- [[config-file|config file]]
- [[toml-config|TOML config]]
- [[configuration-layering|configuration layering]]
- [[environment-profile|environment profile]]
- [[environment-overrides|environment overrides]]
- [[config-deserialization|config deserialization]]
- [[env-var|env::var]]
- [[deserialization]]

## Code Examples

```toml
[dependencies]
config = "0.15"
serde = { version = "1", features = ["derive"] }
```

```toml
[db]
host = "localhost"
port = 5432
login = "postgres"
password = "1111"

[server]
listen_port = 3000
```

```rust
let profile = std::env::var("PROFILE").unwrap_or_else(|_| "dev".into());

let cfg = config::Config::builder()
    .add_source(config::File::with_name("config/default.toml"))
    .add_source(config::File::with_name(&format!("config/{profile}.toml")))
    .add_source(config::Environment::with_prefix("app").separator("__"))
    .build()
    .unwrap();
```

## Exercises or Practice Ideas

- `default.toml` va `dev.toml` bilan DB host override qiling.
- `PROFILE=prod` bilan profile-specific configni tanlang.
- `APP__DB__HOST` yoki source uslubidagi `APP__db__host` orqali nested value override qilib ko'ring.
- Missing config file va invalid type uchun error handlingni `unwrap()`siz yozing.

## Questions Raised

- Profile default `dev` bo'lishi production uchun xavfsizmi?
- Secretlar file, env variable, yoki secret manager orqali kelishi kerakmi?
- Config validation qaysi qatlamda bo'lishi kerak: deserialize paytida, domain constructor’da, yoki startup check’da?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-application-configuration]]
- [[wiki/crates/config]]
- [[wiki/crates/serde]]

