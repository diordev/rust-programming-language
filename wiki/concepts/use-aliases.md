---
title: "Use Aliases"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules]
source_count: 1
---

# Use Aliases

## Short Definition

`use` alias `as` keyword orqali imported item'ga current scope ichida yangi local nom beradi.

## Why It Matters

Bir xil nomli items bitta scope'ga kirsa, Rust qaysi item nazarda tutilganini bilmaydi. Alias collisionni hal qiladi va code'ni aniq qiladi.

## Mental Model

Alias original item'ni o'zgartirmaydi; faqat shu scope ichida ishlatiladigan local nomni beradi.

## Syntax and Examples

```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    Ok(())
}

fn function2() -> IoResult<()> {
    Ok(())
}
```

Bu yerda `std::fmt::Result` qisqa `Result` nomi bilan, `std::io::Result` esa `IoResult` nomi bilan ishlatiladi.

## Common Mistakes

- Aliasni type alias bilan adashtirish; `use ... as ...` faqat import nomini o'zgartiradi.
- Juda umumiy alias tanlab, readabilityni pasaytirish.
- Parent module orqali ajratish yetarli bo'lgan joyda ortiqcha alias ishlatish.

## Related Concepts

- [[use-declarations|use declarations]]
- [[scope]]
- [[paths]]
- [[result|Result]]
- [[readable-code|readable code]]

## Sources

- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
