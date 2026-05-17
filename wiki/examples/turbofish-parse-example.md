---
title: "Turbofish parse example"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, generics, turbofish, parsing]
source_count: 1
---

# Turbofish parse example

Expression context'da generic type argumentni aniq ko'rsatish uchun `::<...>` ishlatiladi.

```rust
let count = "42".parse::<i32>().unwrap();
assert_eq!(count, 42);
```

## Why It Matters

`parse()` bir nechta target type'ga ishlashi mumkin. [[turbofish]] compilerga aynan `i32` kerakligini aytadi.

## Related

- [[turbofish]]
- [[type-inference|type inference]]
- [[functions]]
