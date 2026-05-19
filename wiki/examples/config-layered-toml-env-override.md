---
title: "config layered TOML env override"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, config, configuration, toml]
source_count: 1
---

# config layered TOML env override

Default TOML, profile TOML, va env override ketma-ket source sifatida ulanadi.

```rust
use config::{Config, File};
use serde::Deserialize;

#[derive(Debug, Deserialize, Clone)]
struct AppConfig {
    db: DbConfig,
    server: ServerConfig,
}

#[derive(Debug, Deserialize, Clone)]
struct DbConfig {
    host: String,
    port: u32,
    login: String,
    password: String,
}

#[derive(Debug, Deserialize, Clone)]
struct ServerConfig {
    listen_port: u32,
}

fn load_config() -> Result<AppConfig, config::ConfigError> {
    let profile = std::env::var("PROFILE").unwrap_or_else(|_| "dev".into());

    Config::builder()
        .add_source(File::with_name("config/default.toml"))
        .add_source(File::with_name(&format!("config/{profile}.toml")))
        .add_source(config::Environment::with_prefix("app").separator("__"))
        .build()?
        .try_deserialize()
}
```

## Related

- [[application-configuration|application configuration]]
- [[configuration-layering|configuration layering]]
- [[environment-overrides|environment overrides]]
- [[wiki/crates/config]]

