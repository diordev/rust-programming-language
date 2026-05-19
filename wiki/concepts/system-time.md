---
title: "SystemTime"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, time, std]
source_count: 1
---

# SystemTime

## Short Definition

`std::time::SystemTime` system wall-clock vaqt bilan ishlash API'si.

## Why It Matters

Unix timestamp, file metadata timestamps, audit logs, va external time boundary'lar system clockga bog'liq.

## Mental Model

`SystemTime` real dunyo soatiga yaqin; u OS yoki administrator tomonidan siljitilishi mumkin.

## Syntax and Examples

```rust
let unix = std::time::SystemTime::now()
    .duration_since(std::time::SystemTime::UNIX_EPOCH)?
    .as_secs();
```

## Common Mistakes

- Code execution durationini `SystemTime::now()` difference bilan o'lchash.
- `duration_since` error qaytarishi mumkinligini unutish.

## Related Concepts

- [[unix-time|Unix time]]
- [[instant|Instant]]
- [[duration|Duration]]

## Sources

- [[wiki/sources/rust-for-backend-developers-date-and-time]]

