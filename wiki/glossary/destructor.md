---
title: "destructor"
type: glossary
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, glossary, memory]
source_count: 1
---

# destructor

Object scope'dan chiqib ketganda yoki o'chirilganda chaqiriladigan cleanup funksiyasi. Rustda `Drop` traitining `drop(&mut self)` metodi destructor vazifasini bajaradi va compiler tomonidan avtomatik chaqiriladi.

Related: [[drop|Drop trait]], [[double-free|double free]]

Sources: [[15-3-running-code-on-cleanup-with-the-drop-trait]]
