---
title: "Rust for Backend Developers: Slices"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, slices, chapter]
source_count: 1
---

# Rust for Backend Developers: Slices

## Learning Goal

Slice'ni borrowed subrange sifatida ko'rish va `&[T]`ning API design'dagi afzalligini tushunish.

## Main Ideas

- Slice sequence ustidagi ownershipsiz view.
- Slice pointer + length modeliga ega.
- Mutable slice original collectionning bir qismini o'zgartira oladi.
- `&[T]` ko'pincha `&Vec<T>`dan kengroq va tozaroq interface.

## Concepts

- [[slices]]
- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[array]]
- [[vector|Vec<T>]]

## Examples

```rust
let arr = [0, 1, 2, 3, 4];
let slice = &arr[2..=4];
```

```rust
let mut v = vec![0, 1, 2, 3];
let slice: &mut [i32] = &mut v[1..3];
slice[0] = 9;
```

## Exercises

- `&Vec<T>` qabul qiladigan functionni `&[T]`ga refactor qiling.
- Inclusive va exclusive range bilan ikki slice yozing.

## Review Questions

- Slice nega owner emas?
- `&[T]` va `Vec<T>` orasidagi asosiy boundary nima?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-slices]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[slices]]
- [[reference]]
