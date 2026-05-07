---
title: "Vector Indexing"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, collections, vectors]
source_count: 2
---

# Vector Indexing

## Short Definition

Vector indexing `v[index]` yoki `v.get(index)` orqali [[vector|Vec<T>]] elementiga murojaat qilish.

## Why It Matters

Indexing va `get` out-of-bounds holatda turlicha behavior beradi. Bu API design, [[error-handling|error handling]], va [[memory-safety|memory safety]] tanloviga ta'sir qiladi.

## Mental Model

Vector indexlari 0dan boshlanadi. `&v[2]` uchinchi elementga reference beradi. `v.get(2)` esa `Option<&T>` qaytaradi.

## Syntax and Examples

```rust
let v = vec![1, 2, 3, 4, 5];

let third = &v[2];
let maybe_third = v.get(2);
```

Out-of-bounds:

```rust
let does_not_exist = v.get(100); // None
```

`&v[100]` esa panic qiladi.

Rust bu holatda invalid memory o'qishni davom ettirmaydi; [[buffer-overread|buffer overread]] kabi security risk o'rniga [[panic|panic!]] qiladi.

## Common Mistakes

- Indexlar 1dan boshlanadi deb o'ylash.
- `[]` out-of-bounds bo'lsa `None` qaytaradi deb kutish.
- User-provided index uchun panic qiladigan accessni tanlash.
- `[]` va `get`ni bir xil API deb o'ylash.

## Related Concepts

- [[vector|Vec<T>]]
- [[option|Option]]
- [[panic]]
- [[buffer-overread|buffer overread]]
- [[match]]
- [[borrowing]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
