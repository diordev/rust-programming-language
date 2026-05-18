---
title: "Error Context"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, error-handling, anyhow]
source_count: 1
---

# Error Context

## Short Definition

Original error'ga qo'shimcha matnli description qo'shish, odatda failure qaysi operation yoki qaysi input bilan bog'liqligini ko'rsatish uchun.

## Why It Matters

`No such file or directory` kabi root cause ko'pincha yetarli emas; qaysi fayl yoki qaysi bosqich yiqilganini bilish kerak.

## Mental Model

Root cause o'zgarmaydi, lekin ustiga "bu qaysi ishni qilayotganda bo'ldi" degan label qo'shiladi.

## Syntax and Examples

```rust
use anyhow::Context;

let text = std::fs::read_to_string("non_existing_file.txt")
    .context("Cannot read non_existing_file.txt")?;
```

## Common Mistakes

- Foydasiz generic context yozish.
- Har qatlamda shovqinli duplicate context qo'shish.

## Related Concepts

- [[root-cause]]
- [[wiki/crates/anyhow]]

## Sources

- [[wiki/sources/rust-for-backend-developers-error-handling]]
