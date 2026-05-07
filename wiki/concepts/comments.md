---
title: "Comments"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, comments]
source_count: 1
---

# Comments

## Short Definition

Comments compiler ignore qiladigan, lekin odamlar uchun explanation beradigan source code text.

## Why It Matters

Comments murakkab intent, trade-off, yoki non-obvious logicni tushuntiradi. Rustda idiomatic ordinary comment `//` bilan yoziladi.

## Mental Model

```rust
// This explains why the next line exists.
let lucky_number = 7;
```

## Syntax and Examples

```rust
// Single-line comment
```

```rust
// Longer explanation
// across multiple lines.
```

## Common Mistakes

- Obvious comment yozish: code aytib turgan narsani qayta aytish.
- Commentni code o'zgarganda yangilamaslik.
- Documentation comments bilan ordinary commentsni aralashtirish.

## Related Concepts

- [[documentation-comments|documentation comments]]
- [[readable-code|readable code]]

## Sources

- [[3-4-comments-the-rust-programming-language]]
