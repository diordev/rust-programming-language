---
title: "serde-xml-rs"
type: crate
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, crate, serialization, xml]
source_count: 1
---

# serde-xml-rs

## Short Definition

`serde-xml-rs` serde trait implementationlardan foydalanib XML serialize/deserialize qiladigan crate.

## Cargo Dependency

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde-xml-rs = "0.8.2"
```

## Basic Usage

```rust
let xml = serde_xml_rs::to_string(&employee)?;
let employee: Employee = serde_xml_rs::from_str(xml_text)?;
```

## Notes

- Source snapshot `serde-xml-rs = "0.8.2"`ni ko'rsatadi; latest version deb da'vo qilinmaydi.
- JSON va XML examples bir xil Rust DTO ustida ishlaydi, chunki contract `serde` traitlarida.

## Related Pages

- [[serialization]]
- [[deserialization]]
- [[wiki/crates/serde]]
- [[wiki/sources/rust-for-backend-developers-serialization]]

