---
title: "super Keyword"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules, paths, keywords]
source_count: 1
---

# super Keyword

## Short Definition

`super` relative path'ni parent [[module]]dan boshlash uchun ishlatiladigan Rust keyword.

## Why It Matters

Child module parent module'dagi item'ga murojaat qilishi kerak bo'lsa, `super` pathni module relationshipga bog'laydi. Parent va child birga ko'chirilsa, `super` absolute pathga qaraganda kamroq update talab qilishi mumkin.

## Mental Model

Filesystemdagi `..` parent directoryga chiqishga o'xshaydi. `super::deliver_order()` current module parent'idan `deliver_order` item'ini qidiradi.

## Syntax and Examples

```rust
fn deliver_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::deliver_order();
    }

    fn cook_order() {}
}
```

Bu yerda `fix_incorrect_order` `back_of_house` ichida. `super::deliver_order()` `back_of_house`ning parent module'iga chiqib `deliver_order`ni topadi.

## Common Mistakes

- `super` crate root degani deb o'ylash; u faqat parent module'ga chiqadi.
- Parent item privacy qoidalari baribir amal qilishini unutish.
- `super`ni unrelated module'ga shortcut deb ishlatishga urinish.

## Related Concepts

- [[relative-path|relative path]]
- [[paths]]
- [[module]]
- [[module-tree|module tree]]
- [[scope]]
- [[keywords]]

## Sources

- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
