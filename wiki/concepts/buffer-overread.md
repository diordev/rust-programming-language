---
title: "Buffer Overread"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, memory-safety, security]
source_count: 1
---

# Buffer Overread

## Short Definition

Buffer overread - data structure chegarasidan tashqaridagi memory'ni o'qishga urinish bilan bog'liq security bug.

## Why It Matters

C kabi tillarda out-of-bounds read undefined behaviorga olib kelishi mumkin. Rust esa vector yoki array indexingda bounds check qilib, invalid index bo'lsa [[panic|panic!]] orqali executionni to'xtatadi.

## Mental Model

Program "mening array/vectorimdagi element" deb o'ylab, aslida boshqa memory joyini o'qishi mumkin. Bu private data leak yoki boshqa security vulnerabilityga aylanishi mumkin. Rust bunday holatda "noto'g'ri elementni qaytarish" o'rniga panic qiladi.

## Syntax and Examples

```rust
let v = vec![1, 2, 3];
v[99]; // Rust panic qiladi, out-of-bounds memory'ni o'qimaydi
```

Safer user-facing alternative:

```rust
if let Some(value) = v.get(99) {
    println!("{value}");
}
```

## Common Mistakes

- Panicni memory safety buzildi degani deb o'ylash; bu yerda panic memory safety'ni saqlash mexanizmi.
- User input indexini tekshirmasdan `[]` bilan ishlatish.
- `get` va indexing behaviorini bir xil deb o'ylash.

## Related Concepts

- [[memory-safety|memory safety]]
- [[panic|panic!]]
- [[vector-indexing|vector indexing]]
- [[array]]
- [[option|Option]]

## Sources

- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
