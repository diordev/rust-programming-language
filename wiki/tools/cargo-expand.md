---
title: "cargo expand"
type: tool
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, tool, cargo, macros]
source_count: 2
---

# cargo expand

## Short Definition

`cargo expand` procedural va declarative macro expansiondan keyingi Rust kodini ko'rishga yordam beradigan Cargo subcommand.

## Common Commands

```shell
$ cargo install cargo-expand
$ cargo expand
```

## Notes

- Source'dagi install command noto'g'ri berilgan; to'g'ri install command `cargo install cargo-expand`.
- `serde` derive yoki custom macros nima generatsiya qilayotganini tekshirishda foydali.
- Expanded output o'qish uchun, public API contract o'rniga compiler/macro debugging signali sifatida qaraladi.

## Related Pages

- [[macro]]
- [[procedural-macros|procedural macros]]
- [[derive-attribute|derive attribute]]
- [[wiki/crates/serde]]

## Sources

- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
- [[wiki/sources/rust-for-backend-developers-serialization]]
