---
title: "double free"
type: glossary
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, glossary, memory-safety]
source_count: 1
---

# double free

Bitta xotira blokini ikki marta `free` qilish — undefined behavior va xavfsizlik teshigi (use-after-free + memory corruption). Rust `Drop::drop` ni qo'lda chaqirishni taqiqlab, compiler tomonidan scope oxirida bir marta chaqirilishini ta'minlab, bu xatoni statik darajada bartaraf etadi.

Related: [[drop|Drop trait]], [[destructor]], [[memory-safety|memory safety]]

Sources: [[15-3-running-code-on-cleanup-with-the-drop-trait]]
