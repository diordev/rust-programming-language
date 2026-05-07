---
title: "Backtrace"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, debugging, panic]
source_count: 1
---

# Backtrace

## Short Definition

Backtrace - panic yoki crash nuqtasiga kelguncha chaqirilgan functions ro'yxati.

## Why It Matters

Panic doim ham user code line'ida chaqirilmaydi; library code ichida panic bo'lishi mumkin. Backtrace panicga olib kelgan call chainni ko'rsatib, haqiqiy sababni topishga yordam beradi.

## Mental Model

Backtrace'ni yuqoridan pastga o'qing va birinchi o'zingiz yozgan source file line'ini qidiring. Yuqoridagi lines odatda siz chaqirgan library/core code, pastdagi lines esa sizning code'ingizni chaqirgan runtime frames bo'lishi mumkin.

## Syntax and Examples

```shell
$ RUST_BACKTRACE=1 cargo run
```

Ko'proq ma'lumot kerak bo'lsa:

```shell
$ RUST_BACKTRACE=full cargo run
```

## Common Mistakes

- Backtrace'dagi birinchi standard library line'ini o'z bugingizning joyi deb qabul qilish.
- Debug symbols yo'qligida backtrace nima uchun kam ma'lumot berayotganini tushunmaslik.
- Panic message'dagi location bilan root cause har doim bir xil bo'ladi deb o'ylash.

## Related Concepts

- [[panic|panic!]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[debug-build|debug build]]
- [[release-build|release build]]

## Sources

- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
