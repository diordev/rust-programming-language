---
title: "Feature Flags"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, nightly, unstable-features]
source_count: 1
---

# Feature Flags

## Short Definition

Feature flags — unstable Rust feature'larni nightly'da explicit opt-in bilan yoqadigan mechanism.

## Why It Matters

Bu model new feature'larni real userlar bilan sinashga imkon beradi, lekin stable/beta userlarni unfinished semantics'dan himoya qiladi.

## Mental Model

Feature codebase'ga tushadi, lekin "gate" ortida turadi. Faqat nightly va explicit opt-in gate'ni ochadi.

## Syntax and Examples

Nightly + source-level opt-in talab qilinadi:

```rust
#![feature(example_feature)]
```

## Common Mistakes

- Stable yoki beta'da feature flag ishlaydi deb o'ylash.
- Feature gate bo'lsa, feature production-ready degan xulosa qilish.

## Related Concepts

- [[nightly-rust]]
- [[release-channels]]
- [[rust-rfc-process]]

## Sources

- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7]]
