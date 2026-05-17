---
title: "Simplified vec! Macro Definition"
type: example
status: active
created: 2026-05-09
updated: 2026-05-16
tags: [rust, macros, vector]
source_count: 2
---

# Simplified vec! Macro Definition

## Context

Rust Book Listing 20-35 `vec!` macroning soddalashtirilgan `macro_rules!` definitionini ko'rsatadi. Haqiqiy standard library implementationi allocation optimizatsiyalarini ham qiladi.

## Code

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

Call:

```rust
let v: Vec<u32> = vec![1, 2, 3];
```

Simplified expansion:

```rust
{
    let mut temp_vec = Vec::new();
    temp_vec.push(1);
    temp_vec.push(2);
    temp_vec.push(3);
    temp_vec
}
```

## Explanation

Pattern:

```rust
( $( $x:expr ),* )
```

Meaning:

- `$x:expr` - expression fragment capture.
- `$()` - repeated part group.
- `,` - each expression orasidagi separator.
- `*` - zero or more repetition.

Replacement:

```rust
$(
    temp_vec.push($x);
)*
```

Pattern necha marta match qilsa, body shuncha marta generate bo'ladi. `vec![1, 2, 3]`da `$x` uch marta match qiladi: `1`, `2`, `3`.

Backend beginner source shu idea'ni yana bir soddalashtirilgan variant bilan ko'rsatadi: `0 $( + $rest )*` kabi pattern expansion yoki `vec2!` kabi o'quv macro orqali repetition intuitiv qilinadi.

## Related Concepts

- [[vec-macro|vec! macro]]
- [[declarative-macros|declarative macros]]
- [[macro|macros]]
- [[metaprogramming]]
- [[vector|Vec<T>]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
