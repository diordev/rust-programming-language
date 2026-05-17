---
title: "E0284 type annotations needed"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, types]
source_count: 1
---

# E0284 type annotations needed

## Symptom

Compiler `error[E0284]: type annotations needed` chiqaradi. Chapter 3 examplesida `"42".parse()` result typesi noaniq bo'lgani uchun xato chiqadi.

## Cause

Rust compile time'da variable typeini bilishi kerak. `parse()` ko'p typelarga parse qila oladi, shuning uchun compiler qaysi type kerakligini inference(xulosa) qila olmaydi.

## Fix Pattern

Explicit type annotation bering:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

## Minimal Example

Failing:

```rust
let guess = "42".parse().expect("Not a number!");
```

Fixed:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

## Related Concepts

- [[3-2-data-types]]
- [[data-types|data types]]
- [[type-inference|type inference]]
- [[type-annotations|type annotations]]
- [[result|Result]]
