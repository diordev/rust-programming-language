---
title: "String Literal vs String"
type: comparison
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, string, ownership]
source_count: 2
---

# String Literal vs String

## Short Version

String literal compile time'da known, immutable text. `String` runtime'da heap allocation bilan growable, mutable text.

Rustda string literal type'i `&str`; `String` esa standard librarydagi owned, growable, mutable UTF-8 text collection.

## Comparison

| Feature | String literal | `String` |
| --- | --- | --- |
| Syntax | `"hello"` | `String::from("hello")` |
| Mutability | Immutable | `mut` bilan mutable |
| Memory | Executable ichida hardcoded | Heap allocation |
| Size | Compile time'da known | Runtime'da grow/shrink mumkin |
| Ownership focus | Oddiyroq | Ownership/move/drop uchun asosiy example |
| Common conversion | `literal.to_string()` yoki `String::from(literal)` | `&owned[..]` yoki deref coercion orqali `&str` |

## Example

```rust
let literal = "hello";

let mut owned = String::from("hello");
owned.push_str(", world!");
```

## Related Pages

- [[string-type|String]]
- [[string-slice|String slice]]
- [[utf-8|UTF-8]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[4-1-what-is-ownership]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
