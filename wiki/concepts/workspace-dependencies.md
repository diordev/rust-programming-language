---
title: "Workspace Dependencies"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, cargo, workspaces, dependencies]
source_count: 1
---

# Workspace Dependencies

## Short Definition

`[workspace.dependencies]` workspace root manifestida common external dependency versionlarini markazlash uchun ishlatiladigan Cargo section.

## Why It Matters

Bir nechta workspace member bir xil crate'dan foydalansa, version duplication va drift paydo bo'ladi. Workspace dependency declaration buni root'da bitta joyga yig'adi.

## Mental Model

Root manifest versionni e'lon qiladi, child package esa shu dependency'ni o'z manifestida `workspace = true` bilan ishlatadi. Membership dependency'ni avtomatik bermaydi; child package baribir explicit declaration yozadi.

## Syntax and Examples

```toml
[workspace]
members = ["child_package"]

[workspace.dependencies]
rand = "0.8"
```

```toml
[dependencies]
rand = { workspace = true }
```

## Common Mistakes

- Workspace member bo'lsa dependency avtomatik mavjud deb o'ylash.
- Root'da `[workspace.dependencies]` yozib, child package'da `[dependencies]` ichida `workspace = true` qo'ymaslik.
- Internal `path` dependency va common external dependency'ni bir xil mexanizm deb tushunish.

## Related Concepts

- [[cargo-workspaces]]
- [[dependencies]]
- [[cargo-toml|Cargo.toml]]
- [[package]]

## Sources

- [[wiki/sources/rust-for-backend-developers-workspace-project]]

