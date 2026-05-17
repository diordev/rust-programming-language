---
title: "Rust RFC Process"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, governance, rfc]
source_count: 1
---

# Rust RFC Process

## Short Definition

Rust RFC process — til va ecosystemdagi yirik o'zgarishlar proposal, discussion, acceptance/rejection, implementation, va stabilization orqali o'tadigan governance workflow.

## Why It Matters

Rust featurelari tasodifiy kirmaydi; ular community discussion va team review orqali shakllanadi. Bu release model va stability policy bilan bog'langan.

## Mental Model

RFC -> team discussion -> acceptance/rejection -> implementation behind feature gate -> nightly feedback -> possible stabilization.

## Syntax and Examples

RFC bu code syntax emas, process artifact. Keyingi bosqichdagi feature esa ko'pincha feature gate bilan nightly'ga tushadi.

## Common Mistakes

- RFC accept bo'ldi, demak feature darhol stable bo'ladi deb o'ylash.
- RFC author o'zi albatta implementation qiladi deb o'ylash.

## Related Concepts

- [[feature-flags]]
- [[nightly-rust]]
- [[stability-without-stagnation]]

## Sources

- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7]]
