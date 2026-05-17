---
title: "Range"
type: glossary
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, glossary]
source_count: 3
---

# Range

Ketma-ket values oralig'ini ifodalovchi syntax yoki type, masalan `1..4`. `..` end-exclusive, `..=` esa end-inclusive. Amaliy jihatdan `std::ops::Range` `for` loop uchun qulay syntax bo'lib qolmaydi; u ko'p integer range holatlarida o'zi `Iterator` bo'lib ishlaydi va `next()` bilan ham yurgizilishi mumkin.

Related: [[for-loop|for loop]], [[iterators]]

Sources: [[3-5-control-flow]], [[wiki/sources/rust-for-backend-developers-loops]], [[wiki/sources/rust-for-backend-developers-iterators]]
