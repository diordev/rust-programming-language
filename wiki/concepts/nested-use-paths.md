---
title: "Nested Use Paths"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules]
source_count: 1
---

# Nested Use Paths

## Short Definition

Nested use paths bir xil prefixga ega importsni bitta `use` declaration ichida curly braces bilan birlashtiradi.

## Why It Matters

Katta file'larda ko'p imports vertical space egallaydi. Nested paths related importsni ixcham va o'qilishi osonroq ko'rsatadi.

## Mental Model

Common prefix bir marta yoziladi, farq qiladigan suffixlar `{ ... }` ichida sanaladi.

## Syntax and Examples

```rust
use std::{cmp::Ordering, io};
```

Bu quyidagiga teng:

```rust
use std::cmp::Ordering;
use std::io;
```

Pathlardan biri ikkinchisining prefixi bo'lsa, `self` shu prefix itemning o'zini bildiradi:

```rust
use std::io::{self, Write};
```

Bu `std::io` va `std::io::Write`ni scope'ga olib kiradi.

## Common Mistakes

- Nested import readabilityni yaxshilash o'rniga haddan tashqari murakkab qilish.
- `self`ni current module `self`i bilan chalkashtirish; bu yerda u nested pathdagi prefix itemning o'zini bildiradi.

## Related Concepts

- [[use-declarations|use declarations]]
- [[paths]]
- [[scope]]
- [[readable-code|readable code]]

## Sources

- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
