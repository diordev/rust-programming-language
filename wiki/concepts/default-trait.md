---
title: "Default Trait"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, traits, defaults]
source_count: 1
---

# Default Trait

## Short Definition

`Default` trait'i type uchun standart boshlang'ich qiymatni qaytaradigan `default()` functionini beradi.

## Why It Matters

Config struct'lar, builder-like initialization, `unwrap_or_default`, va `..Default::default()` kabi patternlar shu traitga tayanadi.

## Mental Model

`Default` "bu type uchun reasonable starting value bor" degan signal. Derive bo'lganda har fieldning `default()`i chaqiriladi.

## Syntax and Examples

```rust
#[derive(Default)]
struct Config {
    port: u16,
    verbose: bool,
}

let cfg = Config {
    port: 8080,
    ..Default::default()
};
```

`Option<T>` bilan:

```rust
let items: Option<Vec<i32>> = None;
let owned = items.unwrap_or_default();
```

## Common Mistakes

- `Default` doim biznes jihatdan to'g'ri qiymat beradi deb o'ylash.
- Fieldlardan biri `Default` bo'lmasa derive ishlamasligini unutish.

## Related Concepts

- [[derive-attribute]]
- [[struct-update-syntax]]
- [[option]]
- [[unwrap]]

## Sources

- [[wiki/sources/22-3-c-derivable-traits|22.3]]
