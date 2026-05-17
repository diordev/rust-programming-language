---
title: "Stack Unwinding"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, panic, memory]
source_count: 1
---

# Stack Unwinding

## Short Definition

Stack unwinding - panic paytida Rust call stack bo'ylab ortga yurib, har bir function scope'idagi data cleanup qiladigan default behavior.

## Why It Matters

Unwinding cleanup imkonini beradi, lekin runtime work va binary overhead keltiradi. Kichik binary kerak bo'lgan holatlarda [[abort-on-panic|abort on panic]] tanlanishi mumkin.

## Mental Model

Panic bo'lsa, Rust "qayerga qadar kelgan bo'lsa, shu call stackni teskari tartibda yig'ishtirib chiqadi". Scope'dan chiqayotgan data uchun destructor/drop logic ishlashi mumkin.

## Syntax and Examples

Default behavior alohida config talab qilmaydi:

```rust
fn main() {
    panic!("stop");
}
```

Panic bo'lganda default response stackni unwind qilish va programni tugatishdir.

## Common Mistakes

- Panic doim darhol processni hech qanday cleanup'siz o'chiradi deb o'ylash.
- Unwinding va aborting orasidagi farqni `Result` recovery bilan adashtirish.
- Unwinding memory unsafety degani deb o'ylash.

## Related Concepts

- [[panic|panic!]]
- [[abort-on-panic|abort on panic]]
- [[drop]]
- [[stack-and-heap|stack and heap]]

## Sources

- [[9-1-unrecoverable-errors-with-panic]]
