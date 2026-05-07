---
title: "Correctness"
type: concept
status: stable
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, reliability]
source_count: 1
---

# Correctness

## Short Definition

Correctness — kodning o'ziga mo'ljallangan ishni to'g'ri bajarishi. Rust bu maqsadga ikki yo'l bilan yaqinlashadi: compile time tekshiruvlar (type system, borrow checker) va runtime tekshiruvlar ([[testing|testlar]]).

## Why It Matters

Type system va [[borrow-checker]] katta xato sinfini — noto'g'ri turdagi qiymatlar, dangling pointer, data race — compile time'da rad etadi. Lekin mantiqiy to'g'rilikni, masalan funksiyaning kerakli natija qaytarishini, faqat testlar tasdiqlaydi.

## Related Concepts

- [[testing]]
- [[borrow-checker]]
- [[memory-safety]]
- [[zero-cost-abstractions]]
