---
title: "rustup nightly override example"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, rustup, nightly]
source_count: 1
---

# rustup nightly override example

Stable default'ni saqlab turib, ma'lum project uchun nightly toolchain qo'yish mumkin.

```shell
$ rustup toolchain install nightly
$ cd ~/projects/needs-nightly
$ rustup override set nightly
```

## Why It Matters

Bu workflow global stable ergonomics'ni saqlaydi, lekin alohida projectga [[nightly-rust]] capability beradi.

## Related

- [[rustup]]
- [[nightly-rust]]
- [[release-channels]]
