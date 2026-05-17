---
title: "Raw identifier match function"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, keywords, raw-identifiers, functions]
source_count: 1
---

# Raw identifier match function

`match` Rust keywordi bo'lgani uchun u oddiy function nomi bo'la olmaydi.

### Xato variant

```rust
fn match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}
```

Bu parse error beradi: `expected identifier, found keyword`.

### To'g'ri variant

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

## Why It Matters

`r#` prefiksi keyword'ni identifier sifatida ishlatishga ruxsat beradi. Muhim nuance: `r#` definition va use site'ning ikkalasida ham kerak.

## Related

- [[raw-identifiers]]
- [[keywords]]
- [[match]]
- [[functions]]
