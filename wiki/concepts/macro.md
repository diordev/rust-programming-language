---
title: "Macro"
type: concept
status: needs-review
created: 2026-05-06
updated: 2026-05-06
tags: [rust, macros]
source_count: 2
---

# Macro

## Short Definition

Macro Rustda code yozadigan code mexanizmi; macro call odatda `!` bilan ko'rinadi.

## Why It Matters

`println!` beginner code'dagi birinchi macro bo'lib, function callga o'xshasa ham compile-time expansionga ega.

## Mental Model

Function runtime'da chaqiriladi; macro esa compile jarayonida codega expand bo'lishi mumkin.

## Syntax and Examples

```rust
println!("Hello, world!");
```

## Common Mistakes

- `println!`dagi `!` belgisini unutish.
- Macro va function bir xil deb o'ylash.

## Related Concepts

- [[println-macro|println! macro]]
- [[compiler]]

## Sources

- [[1-2-hello-world-the-rust-programming-language]]
