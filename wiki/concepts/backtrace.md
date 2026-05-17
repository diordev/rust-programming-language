---
title: "Backtrace"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, debugging, panic]
source_count: 2
---

# Backtrace

## Short Definition

Backtrace panic yoki crash nuqtasigacha kelgan call chain.

## Why It Matters

Panic ko'pincha library frame ichida ko'rinadi; root cause esa user code yuqoriroq frame'da bo'ladi.

## Mental Model

Backtrace'ni o'qiganda birinchi o'zingiz yozgan source line'ini qidiring. `RUST_BACKTRACE=1` baseline, `full` ko'proq detal beradi.

Advance panic source yana bir nuance qo'shadi: `catch_unwind` panic'ni ushlasa ham default hook oldin output berishi mumkin. Demak panic log ko'rinishi bilan butun process tugadi deb shoshmaslik kerak.

## Syntax and Examples

```shell
RUST_BACKTRACE=1 cargo run
```

## Common Mistakes

- Birinchi standard library frame'ni root cause deb qabul qilish.
- Hook/output bilan unwind capture'ni aralashtirish.

## Related Concepts

- [[panic]]
- [[panic-hook]]
- [[catch-unwind]]
- [[debug-build|debug build]]

## Sources

- [[9-1-unrecoverable-errors-with-panic]]
- [[wiki/sources/rust-for-backend-developers-panic]]

