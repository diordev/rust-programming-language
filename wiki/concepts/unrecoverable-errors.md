---
title: "Unrecoverable Errors"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, panic]
source_count: 3
---

# Unrecoverable Errors

## Short Definition

Unrecoverable errors - program davom ettirishi to'g'ri bo'lmagan, odatda bug yoki violated invariant belgisi bo'lgan xatolar.

## Why It Matters

Rust bunday holatlarda invalid memory access yoki undefined behaviorga o'tmaydi. [[panic|panic!]] executionni to'xtatib, bugni ko'rinadigan qiladi.

## Mental Model

Unrecoverable error "bu pathda valid result yo'q" degani. Masalan vector ichida mavjud bo'lmagan indexga `[]` bilan murojaat qilish: `[]` element qaytarishi kerak, lekin qaytariladigan element yo'q.

9.3 language bilan aytganda, unrecoverable holatlar ko'pincha [[bad-state|bad state]], broken [[function-contracts|contract]], yoki broken [[invariants|invariant]] bilan bog'liq.

## Syntax and Examples

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99]; // panic
}
```

Bevosita panic:

```rust
panic!("crash and burn");
```

## Common Mistakes

- Har qanday errorni unrecoverable deb ko'rish.
- Panicni normal user-facing error handling o'rniga ishlatish.
- Panic outputidagi library line'ni haqiqiy sabab deb qabul qilib, [[backtrace]]dan user code line'ini qidirmaslik.
- Expected failuresni unrecoverable deb belgilash.

## Related Concepts

- [[error-handling]]
- [[panic|panic!]]
- [[recoverable-errors|recoverable errors]]
- [[backtrace]]
- [[vector-indexing|vector indexing]]
- [[bad-state|bad state]]
- [[invariants]]
- [[function-contracts|function contracts]]

## Sources

- [[9-error-handling-the-rust-programming-language]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
