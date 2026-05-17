---
title: "Absolute Path"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules, paths]
source_count: 1
---

# Absolute Path

## Short Definition

Absolute path item'ga [[crate-root|crate root]]dan boshlanib boradigan full [[paths|path]].

## Why It Matters

Absolute path module tree ichida item qayerda joylashganini aniq ko'rsatadi. Current crate ichida u `crate` bilan boshlanadi; external crate uchun external crate nomi bilan boshlanadi.

## Mental Model

Filesystemda `/` rootdan boshlashga o'xshaydi. Rustda current crate uchun root nomi `crate`.

## Syntax and Examples

```rust
crate::front_of_house::hosting::add_to_waitlist();
```

Bu path `crate` rootidan boshlanib, `front_of_house`, keyin `hosting`, keyin `add_to_waitlist` functioniga boradi.

## Common Mistakes

- Absolute path privacy qoidalarini chetlab o'tadi deb o'ylash.
- Current crate ichida absolute path `src` yoki package nomidan boshlanadi deb taxmin qilish.
- Code ko'chirilganda absolute path hech qachon o'zgarmaydi deb o'ylash; target module ko'chsa path ham yangilanadi.

## Related Concepts

- [[paths]]
- [[relative-path|relative path]]
- [[crate-root|crate root]]
- [[module-tree|module tree]]
- [[privacy]]

## Sources

- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
