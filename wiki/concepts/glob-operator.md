---
title: "Glob Operator"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules]
source_count: 1
---

# Glob Operator

## Short Definition

Glob operator `*` path ichidagi hamma public item'larni current scope'ga olib kiradigan `use` shakli.

## Why It Matters

Glob imports kamroq yozuv bilan ko'p item'ni olib kiradi, lekin qaysi nom qayerdan kelganini noaniq qilishi va dependency update'larida name collision yaratishi mumkin.

## Mental Model

`use path::*;` "shu public namespace ichidagi hamma narsani shu scope'ga olib kir" degani. Bu explicit importsga qaraganda kuchliroq, lekin kamroq aniq.

## Syntax and Examples

```rust
use std::collections::*;
```

Testingda ba'zan tested module'dagi hamma public item'larni `tests` module ichiga olib kirish uchun glob ishlatiladi.

## Common Mistakes

- Production code'da glob importni odat qilish.
- Glob import dependencydagi yangi public item'lar scope'ingizga ham kirishini unutish.
- Qaysi name qayerdan kelganini reviewda topishni qiyinlashtirish.

## Related Concepts

- [[use-declarations|use declarations]]
- [[scope]]
- [[paths]]
- [[prelude]]
- [[testing]]

## Sources

- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
