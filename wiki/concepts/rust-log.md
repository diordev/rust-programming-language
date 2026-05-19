---
title: "RUST_LOG"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, logging, environment-variables]
source_count: 1
---

# RUST_LOG

## Short Definition

Rust logging ecosystemida runtime log filter directives berish uchun ishlatiladigan environment variable.

## Why It Matters

Log verbosityni binary rebuild qilmasdan sozlash kerak.

## Mental Model

Application `EnvFilter::from_default_env()` ishlatsa, `RUST_LOG` qiymati filter policy bo'ladi.

## Syntax and Examples

```shell
$ export RUST_LOG="myapp::mod_a=debug,myapp::mod_b=warn,info"
$ cargo run
```

## Common Mistakes

- App ichida `EnvFilter::from_default_env()` ulanmagan bo'lsa, `RUST_LOG`dan natija kutish.
- Productionda juda keng `trace` yoqish.

## Related Concepts

- [[env-filter|EnvFilter]]
- [[env-var|env::var]]
- [[log-levels|log levels]]

## Sources

- [[wiki/sources/rust-for-backend-developers-logging]]

