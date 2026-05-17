---
title: "Comments"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-13
tags: [rust, comments]
source_count: 2
---

# Comments

## Short Definition

Comments compiler ignore qiladigan, lekin odamlar uchun explanation beradigan source code text.

## Why It Matters

Comments murakkab intent, trade-off, yoki non-obvious logicni tushuntiradi. Rustda idiomatic ordinary comment `//` bilan yoziladi.

Appendix B comment syntax oilasini bitta jadvalga yig'adi: ordinary comments, inner/outer line doc comments, va inner/outer block doc comments.

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

Doc comment oilasi:

```rust
/// outer line doc comment
//! inner line doc comment
/** outer block doc comment */
/*! inner block doc comment */
```

## Common Mistakes

- Obvious comment yozish: code aytib turgan narsani qayta aytish.
- Commentni code o'zgarganda yangilamaslik.
- Documentation comments bilan ordinary commentsni aralashtirish.

## Related Concepts

- [[documentation-comments|documentation comments]]
- [[readable-code|readable code]]
- [[rust-operators-and-symbols]]

## Sources

- [[3-4-comments]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
