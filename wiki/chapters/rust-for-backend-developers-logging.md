---
title: "Rust for Backend Developers: Logging"
type: chapter
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, logging, tracing, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Logging

## Learning Goal

Backend app uchun `tracing` ecosystemi bilan level, filter, format, writer, layer, va span contextlarini aniq sozlash.

## Main Ideas

- [[wiki/crates/tracing|tracing]] logging API/facade, [[wiki/crates/tracing-subscriber|tracing-subscriber]] subscriber implementation.
- `tracing_subscriber::fmt().init()` global dispatcherga formatting subscriber register qiladi.
- Format options log outputni console, file, yoki JSON structured logging uchun moslaydi.
- [[env-filter|EnvFilter]] va [[rust-log|RUST_LOG]] runtime log filteringning asosiy patterni.
- [[wiki/crates/tracing-appender|tracing-appender]] rolling file va non-blocking writer beradi.
- [[tracing-layer|Layers]] bir nechta output va formatting policy'ni bitta registryga birlashtiradi.
- [[tracing-span|Span]] log eventlarga request/function/context fieldlarini bog'laydi; `#[instrument]` function body uchun span yaratadi.
- Async code’da active entered span va `.await` boundary ehtiyotkorlik talab qiladi.

## Concepts

- [[logging]]
- [[structured-logging|structured logging]]
- [[log-levels|log levels]]
- [[subscriber]]
- [[env-filter|EnvFilter]]
- [[rust-log|RUST_LOG]]
- [[log-target|log target]]
- [[log-writer|log writer]]
- [[non-blocking-logging|non-blocking logging]]
- [[worker-guard|WorkerGuard]]
- [[tracing-layer|tracing layer]]
- [[tracing-registry|tracing registry]]
- [[tracing-span|tracing span]]
- [[entered-span|EnteredSpan]]
- [[instrument-attribute|#[instrument]]]

## Examples

```rust
tracing_subscriber::fmt()
    .with_env_filter(tracing_subscriber::EnvFilter::from_default_env())
    .json()
    .init();
```

```rust
#[tracing::instrument(skip(password), fields(user_id = %user_id))]
fn login(user_id: u64, password: &str) {
    tracing::info!("login attempt");
}
```

## Exercises

- `RUST_LOG="myapp::db=debug,info"` filterini ishlatib ko'ring.
- File va stdout outputni alohida layer bilan ulang.
- Non-blocking appender guard'ini `main` oxirigacha tirik saqlang.
- `#[instrument]` bilan function argumentlaridan sensitive fieldni `skip` qiling.

## Review Questions

- `tracing` facade bilan subscriber implementation orasidagi farq nima?
- `with_max_level` va `EnvFilter` qaysi holatda farq qiladi?
- Nega file output uchun ANSI o'chiriladi?
- `WorkerGuard` nima uchun `_guard` sifatida scope'da saqlanadi?
- Span nesting qachon parent-child relation yaratadi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-logging]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/crates/tracing]]
- [[wiki/crates/tracing-subscriber]]
- [[wiki/crates/tracing-appender]]

