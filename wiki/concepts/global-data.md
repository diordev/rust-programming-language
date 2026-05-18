---
title: "Global Data"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, globals, backend]
source_count: 1
---

# Global Data

## Short Definition

Program bo'ylab accessible bo'lgan `const`, `static`, yoki lock bilan himoyalangan shared state.

## Why It Matters

Config, registry, cache, session store kabi value'lar ba'zan global joylashadi, lekin bunday tanlov concurrency va initialization tradeoff olib keladi.

## Mental Model

Global data convenience beradi, lekin ownership va synchronization masalasini yo'qotmaydi; uni bitta markazga to'playdi.

## Syntax and Examples

```rust
static COUNTER: std::sync::Mutex<u64> = std::sync::Mutex::new(0);
```

## Common Mistakes

- Global bo'lsa osonroq deb default tanlash.
- Init semantics va lock contentionni e'tiborsiz qoldirish.

## Related Concepts

- [[global-state]]
- [[constants]]
- [[static-items]]
- [[lazylock|LazyLock]]
- [[oncelock|OnceLock]]

## Sources

- [[wiki/sources/rust-for-backend-developers-global-data]]
