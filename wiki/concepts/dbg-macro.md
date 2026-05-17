---
title: "dbg! Macro"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, macros, debug]
source_count: 1
---

# dbg! Macro

## Short Definition

`dbg!` expression value'sini file va line number bilan debug outputga chiqaradigan macro.

## Why It Matters

`dbg!` quick debugging uchun qulay: expressionni evaluate qiladi, value'ni [[stderr]]ga chiqaradi, keyin ownershipni qaytaradi.

## Mental Model

`dbg!(expr)` expression ownershipini oladi va qaytaradi. Agar value'ni keyin ham ishlatmoqchi bo'lsangiz, reference berish mumkin: `dbg!(&rect1)`.

## Syntax and Examples

```rust
let scale = 2;
let rect1 = Rectangle {
    width: dbg!(30 * scale),
    height: 50,
};

dbg!(&rect1);
```

## Common Mistakes

- `dbg!(rect1)` ownershipni move qilishi mumkinligini unutish.
- `dbg!` output [[stdout]]ga chiqadi deb o'ylash; u [[stderr]]ga yozadi.

## Related Concepts

- [[macro]]
- [[debug-formatting|Debug formatting]]
- [[debug-trait|Debug trait]]
- [[ownership]]
- [[borrowing]]

## Sources

- [[5-2-an-example-program-using-structs]]
