---
title: "Compiler"
type: tool
status: needs-review
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler, tool]
source_count: 2
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

## Related Pages

- [[rustc]]
- [[cargo|Cargo]]
- [[ownership]]
- [[type-inference|type inference]]

## Sources

- [[0-1-foreword-the-rust-programming-language]]
- [[1-getting-started|1. Getting Started]]
