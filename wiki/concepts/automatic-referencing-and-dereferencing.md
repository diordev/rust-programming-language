---
title: "Automatic Referencing and Dereferencing"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, methods, borrowing]
source_count: 1
---

# Automatic Referencing and Dereferencing

## Short Definition

Method callsda Rust receiverni method signaturega moslashtirish uchun kerakli `&`, `&mut`, yoki `*`ni avtomatik qo'shishi.

## Why It Matters

Bu Rustda C/C++dagi `->` operatorni keraksiz qiladi va method syntaxni ownership model bilan ergonomic qiladi.

## Mental Model

`object.method()` yozilganda Rust methodning `self` receiver type'iga qarab objectni borrow, mutable borrow, yoki deref qiladi.

## Syntax and Examples

```rust
p1.distance(&p2);
(&p1).distance(&p2);
```

Bu ikki call method receiver uchun equivalent bo'lishi mumkin.

## Common Mistakes

- Rustda method call uchun `->` operator izlash.
- Automatic behavior hamma expression contextlarida ishlaydi deb o'ylash; bu method receiverlarga xos.

## Related Concepts

- [[method-syntax|method syntax]]
- [[borrowing]]
- [[reference]]
- [[deref-coercions|deref coercions]]

## Sources

- [[5-3-methods]]
