---
title: "Relative Path"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules, paths]
source_count: 1
---

# Relative Path

## Short Definition

Relative path current [[module]]dan boshlanadigan [[paths|path]] bo'lib, `self`, [[super-keyword|super]], yoki current module'dagi identifier bilan yoziladi.

## Why It Matters

Related code birga ko'chishi ehtimoli yuqori bo'lsa, relative path refactor paytida kamroq o'zgarishi mumkin. U current module contextiga tayanadi, shuning uchun local organizationni ifodalaydi.

## Mental Model

Filesystemda current directorydan boshlanadigan path kabi. Current module qayerda turganiga qarab path ma'nosi o'zgaradi.

## Syntax and Examples

```rust
front_of_house::hosting::add_to_waitlist();
```

`eat_at_restaurant` crate rootda turganda bu relative path `front_of_house` sibling module'idan boshlanadi.

Parent module'dan boshlash:

```rust
super::deliver_order();
```

## Common Mistakes

- Relative pathni code boshqa module'ga ko'chganda ham avtomatik to'g'ri qoladi deb o'ylash.
- `super` parent module'ga chiqishini unutish.
- Relative path ham privacy qoidalariga bo'ysunishini unutish.

## Related Concepts

- [[paths]]
- [[absolute-path|absolute path]]
- [[super-keyword|super keyword]]
- [[module]]
- [[module-tree|module tree]]
- [[privacy]]

## Sources

- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
