---
title: "unimplemented!"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, macros, panic]
source_count: 1
---

# unimplemented!

## Short Definition

`unimplemented!` hali yozilmagan logic uchun panic qiluvchi placeholder macro.

## Why It Matters

`todo!`ga juda o'xshaydi, lekin ma'noda keyinroq implement qilinishi mumkin bo'lgan branchni belgilaydi.

## Mental Model

Semantik farq bor, mexanik farq deyarli yo'q: ikkisi ham panic qiladi va `!` qaytaradi.

## Syntax and Examples

```rust
fn parse() -> u32 {
    unimplemented!()
}
```

## Common Mistakes

- `todo!` bilan behavior farqi bor deb o'ylash.

## Related Concepts

- [[todo-macro|todo!]]
- [[never-type|never type (!)]]

## Sources

- [[wiki/sources/rust-for-backend-developers-panic]]

