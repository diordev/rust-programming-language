---
title: "Dependencies"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, cargo]
source_count: 4
---

# Dependencies

## Short Definition

`dependencies` project ishlatadigan external crates yoki libraries ro'yxati.

## Why It Matters

Cargo dependenciesni download, build, version resolve, va lock qilish workflowini boshqaradi.

## Mental Model

`Cargo.toml` kerakli crates va version constraintsni aytadi; [[cargo-lock|Cargo.lock]] exact resolved versionsni saqlaydi.

Package kattalashsa, ayrim qismlar alohida [[crate]] sifatida chiqarilib, keyin external dependency sifatida ishlatilishi mumkin.

External crate ishlatilishi uchun dependency manifestda ko'rsatiladi, keyin code ichida crate nomidan boshlanadigan path yoki [[use-declarations|use declaration]] ishlatiladi.

## Syntax and Examples

```toml
[dependencies]
rand = "0.8.5"
```

```rust
use rand::Rng;
```

## Common Mistakes

- `Cargo.lock` va `Cargo.toml` vazifalarini aralashtirish.
- Version constraint va exact selected version bir xil deb o'ylash.
- `Cargo.toml`ga dependency qo'shilsa, kerakli trait yoki item avtomatik scope'ga kiradi deb o'ylash.

## Related Concepts

- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[package]]
- [[crate]]
- [[semver|SemVer]]
- [[use-declarations|use declarations]]
- [[rand]]

## Sources

- [[1-3-hello-cargo]]
- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/sources/7-packages-crates-and-modules]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
