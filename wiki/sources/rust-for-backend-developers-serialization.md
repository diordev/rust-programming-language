---
title: "Сериализация - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, source, serialization, serde]
source_count: 1
---

# Сериализация - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/55. Сериализация.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/serialization.html

## Detailed Summary

Bu source Rust standard library'da built-in [[serialization]] yo'qligini, amaliy standart esa [[wiki/crates/serde|serde]] ekanini ko'rsatadi. `serde` format implementation emas; u `Serialize` va `Deserialize` traitlari, derive macros, va type metadata layerini beradi. JSON, XML yoki boshqa formatlar esa shu trait implementationlardan foydalanadigan alohida crate'lar orqali ishlaydi.

`Employee { id, name }` misoli `serde = { version = "1", features = ["derive"] }` dependency bilan boshlanadi. `#[derive(Serialize, Deserialize)]` struct uchun serialization/deserialization code generatsiya qiladi. Source `cargo expand` orqali macro expansionni ko'rishni taklif qiladi, lekin command caveat bor: to'g'ri install command `cargo install cargo-expand`. Generated `Serialize` implementation oddiyroq: struct nomi va field metadata'ni serializerga beradi. `Deserialize` esa murakkabroq: field visitor, duplicate/missing field handling, map/seq access kabi qismlar generatsiya qilinadi.

JSON bo'limi [[wiki/crates/serde-json|serde_json]]ni format crate sifatida ishlatadi. `serde_json::to_string` Rust value'ni JSON stringga, `serde_json::from_str` JSON stringni typed Rust value'ga aylantiradi. Bu source uchun asosiy mental model: typed domain struct bilan ishlash default, raw JSON tree bilan ishlash esa `serde_json::Value` kerak bo'lganda tanlanadi.

`serde_json::Value` JSON typelarini Rust enum orqali ifodalaydi: `Null`, `Bool`, `Number`, `String`, `Array`, `Object`. Bu dynamic yoki oldindan schema aniq bo'lmagan JSON bilan ishlashda foydali, lekin typed modeldagi compile-time guaranteesni kamaytiradi. Source `Value`ni `match` bilan qo'lda ajratish misolini beradi.

XML bo'limi [[wiki/crates/serde-xml-rs|serde-xml-rs]] orqali bir xil `Serialize`/`Deserialize` modelining XML formatga ham ishlashini ko'rsatadi. Bu serde'ning asosiy kuchi: domain type bir marta derive qilinadi, format crate'lari esa turli wire formatlarga output/input beradi.

Enum serialization bo'limi backend wire format design uchun eng muhim qism. Default strategy externally tagged: variant nomi tashqi object key sifatida chiqadi. `#[serde(tag = "type")]` internally tagged strategy bo'lib, variant nomini object ichidagi fieldda saqlaydi, lekin tuple variants bilan ishlamaydi. `#[serde(tag = "type", content = "obj")]` adjacently tagged strategy bo'lib, tag va payloadni alohida fieldlarda saqlaydi. `#[serde(untagged)]` tag qo'shmaydi, lekin shape ambiguity sababli deserialization xavfli bo'lishi mumkin.

Field rename bo'limi `#[serde(rename = "name")]` orqali Rust field nomi bilan serialized field nomini ajratadi. Naming convention bo'limi `#[serde(rename_all = "camelCase")]` kabi container-level mappingni ko'rsatadi. Source'dagi field typo tuzatildi: `first_name` uchun camelCase field `firstName` bo'lishi kerak.

Oxirgi bo'lim [[deserializeowned|DeserializeOwned]] haqida. `Deserialize<'de>` input data lifetime'iga bog'langan bo'lishi mumkin; generic holder kabi type ichida `'de` faqat trait boundda ishlatilsa muammo chiqadi. `DeserializeOwned` `for<'de> Deserialize<'de>` orqali "input buffer'dan borrow qilmaydigan owned result" talabini ifodalaydi. Bu advanced boundary: generic helpers, cache, queue, yoki request body'dan owned value olishda foydali.

## Key Concepts

- [[serialization]]
- [[deserialization]]
- [[wiki/crates/serde|serde]]
- [[serialize-trait|Serialize trait]]
- [[deserialize-trait|Deserialize trait]]
- [[serde-json-value|serde_json::Value]]
- [[serde-enum-tagging]]
- [[serde-rename]]
- [[deserializeowned|DeserializeOwned]]
- [[higher-ranked-trait-bounds|higher-ranked trait bounds]]
- [[procedural-macros|procedural macros]]
- [[cargo-expand|cargo expand]]

## Code Examples

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

```rust
use serde::{Deserialize, Serialize};

#[derive(Debug, Serialize, Deserialize)]
struct Employee {
    id: u64,
    name: String,
}
```

```rust
let json = serde_json::to_string(&emp).unwrap();
let emp: Employee = serde_json::from_str(r#"{"id":1,"name":"John"}"#).unwrap();
```

```rust
#[derive(Debug, Serialize, Deserialize)]
#[serde(tag = "type", content = "obj")]
enum Employee {
    Programmer { name: String, language: String },
    Manager { name: String },
    OfficeCat(String),
}
```

## Exercises or Practice Ideas

- `Employee` structini JSON va XML formatlarga serialize/deserialize qilib ko'ring.
- Bitta enum uchun externally tagged, internally tagged, adjacently tagged, va untagged JSON outputlarni solishtiring.
- `rename_all = "camelCase"` bilan backend API DTO yozing va `first_name` fieldi `firstName` bo'lib chiqishini tekshiring.
- `DeserializeOwned` bilan generic `parse_body<T>()` helper yozib ko'ring.

## Questions Raised

- Backend public API'da qaysi enum tagging strategy eng barqaror wire contract beradi?
- `serde_json::Value` qachon kerak, qachon typed struct yaxshiroq?
- `DeserializeOwned` talab qilish borrowed deserialization performance imkoniyatlarini qayerda cheklaydi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-serialization]]
- [[wiki/crates/serde]]
- [[wiki/crates/serde-json]]
- [[wiki/crates/serde-xml-rs]]
