---
title: "Compiler-Driven Development"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, compiler, workflow, design]
source_count: 1
---

# Compiler-Driven Development

## Short Definition

Avval ishlatmoqchi bo'lgan API yoki code shape'ni yozib, keyin compiler xatolari ko'rsatgan navbatdagi yetishmayotgan qismlarni birma-bir qurish usuli.

## Why It Matters

Rust compiler juda kuchli feedback beradi: yo'q type, yo'q method, noto'g'ri ownership, yetishmayotgan trait bound. Shu sababli katta design'ni birdan mukammal yozish o'rniga, desired interface'dan boshlab implementation'ni compiler ko'rsatgan yo'nalishda yetishtirish samarali bo'ladi.

## Mental Model

Bu "compiler bilan dialog". Siz maqsad API'ni yozasiz, compiler esa: "endi `ThreadPool` yo'q", "endi `new` yo'q", "endi `execute` yo'q", "endi `receiver` moved bo'ldi" deb aytadi. Har xato navbatdagi design qaroriga aylanishi mumkin.

## Syntax and Examples

### Desired API first

```rust
let pool = ThreadPool::new(4);

pool.execute(|| {
    handle_connection(stream);
});
```

### Keyingi compiler feedback

- `use of undeclared type ThreadPool`
- `no function or associated item named new`
- `no method named execute`
- `use of moved value: receiver`

## Common Mistakes

- **Compiler xatosini faqat obstacle deb ko'rish**: ko'pincha u design hint beradi.
- **Interface haqida o'ylamasdan ichki implementationdan boshlash**: keyin API noqulay bo'lib qolishi mumkin.
- **Compiler-driven developmentni test o'rniga ishlatish**: compile bo'lishi yetarli emas, xulq ham tekshirilishi kerak.

## Related Concepts

- [[compiler]]
- [[tdd]]
- [[thread-pool]]
- [[fn-traits|Fn traits]]
- [[ownership]]

## Sources

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
