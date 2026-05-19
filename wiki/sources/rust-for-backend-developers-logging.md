---
title: "Логирование - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, source, logging, tracing]
source_count: 1
---

# Логирование - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/57. Логирование.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/logging.html

## Detailed Summary

Bu source backend logging uchun [[wiki/crates/tracing|tracing]] ecosystemini tanlaydi. `tracing` API/facade beradi, [[wiki/crates/tracing-subscriber|tracing-subscriber]] esa eventlarni qabul qilib formatlash, filterlash va yozish implementationini beradi. Source `log`, `env_logger`, `log4rs` borligini eslatadi, lekin async backend uchun `tracing`ga fokus qiladi.

Birinchi misol `tracing_subscriber::fmt().init()` va `tracing::info!("Hello")` bilan boshlanadi. `fmt().init()` aslida builder yaratish, subscriber qurish, va global dispatcherga register qilish shortcutidir. `tracing::info!` eventni global dispatcherga yuboradi; dispatcher uni registered subscriberga beradi.

Subscriber ichki modeli source'da generic fieldlar orqali ko'rsatiladi: fields formatting, event format, level filter, va writer. Amaliy xulosa: logging faqat `println!` emas; format, target, level, writer, timer, va layers bilan boshqariladigan observability boundary.

Format bo'limi `SubscriberBuilder` optionsni beradi: ANSI color, file/line, target, thread id/name, timer, `without_time`, va `json()`. Console uchun ANSI qulay, file yoki JSON output uchun ANSI o'chiriladi. JSON logging `json` feature bilan ishlaydi va structured output beradi.

Level bo'limi `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`ni ajratadi. Default output `INFO` va undan yuqori level eventlarini ko'rsatadi. `with_max_level(LevelFilter::DEBUG)` kabi sozlama global thresholdni pasaytirishi mumkin, lekin real backendda dependency shovqinini filterlash kerak bo'ladi.

Filter bo'limi [[env-filter|EnvFilter]]ni ko'rsatadi. Filter string `target::module=level` sectionlaridan tuziladi; `RUST_LOG` orqali runtime’da boshqarish Rust ecosystemida odatiy pattern. Source matnida bir joyda `mod1` ikki marta tilga olinadi; wiki'da ikkinchi module `mod2` deb tuzatiladi.

File logging bo'limi `with_writer` va `std::io::Write` orqali primitive file loggingni ko'rsatadi. Long-running backend uchun [[wiki/crates/tracing-appender|tracing-appender]] afzalroq: rolling files, retention, va non-blocking writer. Source `rolling::daily("logs", "app.log")` code'ini beradi; bu daily rotation, source matnidagi rotation tavsifi code bilan mos emas.

Non-blocking bo'limi `tracing_appender::non_blocking`ni tushuntiradi. U writer ortida alohida worker thread va channel ishlatadi. `WorkerGuard` drop bo'lganda queue'dagi loglarni flush qilishga yordam beradi. Muhim caveat: queue to'lsa yangi loglar drop qilinishi mumkin; default buffer source bo'yicha 128000 log message.

Layer bo'limi bitta subscriber ichida bir nechta output/filter/formatter qatlamini birlashtirishni ko'rsatadi. `tracing_subscriber::registry().with(stdout_layer).with(file_layer).init()` patterni console va file outputni alohida sozlashga imkon beradi.

Span bo'limi `tracing`ning logdan ko'ra kuchliroq tomoni. [[tracing-span|Span]] code regionga context fieldlar qo'shadi. `enter()` borrowed `Span`ga tied guard qaytaradi; temporary span ustida `enter()` qilish lifetime muammosiga olib keladi. `entered()` esa span ownershipni guard ichiga oladigan `EnteredSpan` beradi.

Span nesting source'da parent-child relation orqali tushuntiriladi: already active span ichida yaratilgan yangi span child bo'ladi; oldindan yaratilgan span keyin enter qilinsa parent relation avtomatik o'rnatilmaydi. `parent: &span1` explicit parent beradi. `#[tracing::instrument]` function body'ni span bilan o'raydi, argumentlarni field sifatida qo'shadi, `skip`, `fields`, `ret`, `name`, `level` options beradi.

Oxirgi warning muhim: async code ichida active entered span scope'ida `.await` ishlatish noto'g'ri contextga olib kelishi mumkin. Wiki bu caveatni keyingi async-focused material bilan bog'lash uchun saqlaydi.

## Key Concepts

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
- [[thread-local-storage|thread local storage]]

## Code Examples

```toml
[dependencies]
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["env-filter", "time", "json"] }
tracing-appender = "0.2"
```

```rust
fn main() {
    tracing_subscriber::fmt().init();
    tracing::info!("Hello");
}
```

```rust
tracing_subscriber::fmt()
    .with_env_filter(tracing_subscriber::EnvFilter::from_default_env())
    .init();
```

```rust
#[tracing::instrument(skip(secret), fields(user_id = %user_id))]
fn handle_request(user_id: u64, secret: &str) {
    tracing::info!("request started");
}
```

## Exercises or Practice Ideas

- `RUST_LOG` orqali bitta module uchun `debug`, butun app uchun `info` filterini yozing.
- JSON log output va plain text outputni solishtiring.
- File appender va stdout layerni alohida qilib registry bilan ulang.
- `#[instrument]`da sensitive argumentni `skip` qilib, custom field qo'shing.

## Questions Raised

- Backend production defaulti plain text logmi yoki JSON structured logmi?
- Non-blocking loggingda queue overflow bo'lsa log drop qilish qabul qilinadimi?
- Async functionlarda span contextni qanday to'g'ri propagate qilish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-logging]]
- [[wiki/crates/tracing]]
- [[wiki/crates/tracing-subscriber]]
- [[wiki/crates/tracing-appender]]
