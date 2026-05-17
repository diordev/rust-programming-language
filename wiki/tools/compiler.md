---
title: "Compiler"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-13
tags: [rust, compiler, tool]
source_count: 3
---

# Compiler

## Short Definition

Rust compiler source code'ni tekshiradi va executable yoki library artifactga aylantiradi.

## Common Commands

```shell
$ rustc main.rs
$ cargo check
$ cargo build
```

## Notes

- Compiler ownership, types, lifetimes, va borrowing qoidalarini compile time'da tekshiradi.
- Beginner workflowda compiler diagnostics learning feedback sifatida ishlatiladi.
- Direct compiler command `rustc`, normal project workflow esa [[cargo|Cargo]] orqali yuradi.
- Compiler version va [[edition]] alohida narsalar: compiler yangi bo'lishi mumkin, lekin crate eski edition qoidalari bilan parse qilinadi.
- Rust compiler release train orqali [[release-channels|stable]], [[release-channels|beta]], va [[nightly-rust|nightly]] channelsda tarqatiladi.

## Related Pages

- [[rustc]]
- [[cargo|Cargo]]
- [[ownership]]
- [[type-inference|type inference]]
- [[edition]]
- [[release-channels]]

## Sources

- [[0-1-foreword]]
- [[wiki/chapters/1-getting-started|1. Getting Started]]
- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7. G - How Rust is Made and Nightly Rust]]
