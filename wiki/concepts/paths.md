---
title: "Paths"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules]
source_count: 4
---

# Paths

## Short Definition

Path Rust item'ini — masalan function, struct, enum, yoki module'ni — nomlash yo'li.

## Why It Matters

Katta codebase'da item'lar turli module va scope'larda yashaydi. Path compiler va programmerga qaysi item nazarda tutilayotganini aniq aytadi.

## Mental Model

Path [[module-tree|module tree]] ichida item'ga boradigan manzilga o'xshaydi. Path ishlashi uchun item crate ichida mavjud bo'lishi va [[privacy]] qoidalari ruxsat berishi kerak.

Path ikki shaklda bo'ladi:

- [[absolute-path|Absolute path]] crate rootdan boshlanadi.
- [[relative-path|Relative path]] current module'dan boshlanadi.

[[use-declarations|use declarations]] path'ni current scope'ga shortcut sifatida olib kiradi. Bu pathning asl joyini yoki privacy qoidalarini o'zgartirmaydi.

## Syntax and Examples

```rust
std::cmp::Ordering
```

Bu standard library ichidagi `Ordering` enumiga boradigan path.

```rust
crate::garden::vegetables::Asparagus
```

Bu crate rootdan boshlanib `garden`, keyin `vegetables` module'i ichidagi `Asparagus` type'iga boradigan path.

```rust
crate::front_of_house::hosting::add_to_waitlist();
front_of_house::hosting::add_to_waitlist();
```

Birinchisi absolute path; ikkinchisi current module'dan boshlanadigan relative path.

```rust
super::deliver_order();
```

Bu relative path parent module'dan boshlanadi.

```rust
use std::{cmp::Ordering, io};
```

Bu nested `use` path common `std` prefixini bir marta yozadi.

## Common Mistakes

- Path va filesystem path'ni aynan bir xil mexanizm deb o'ylash.
- Item scope'da bo'lmasa ham qisqa nom bilan ishlatish mumkin deb kutish.
- Path to'g'ri bo'lsa privacy har doim ruxsat beradi deb o'ylash.
- Absolute path har doim refactorda kamroq update talab qiladi deb o'ylash; target module ko'chsa absolute path ham o'zgaradi.
- `use` path'ni boshqa scope'larda ham valid qiladi deb kutish.

## Related Concepts

- [[module-system|module system]]
- [[module]]
- [[module-tree|module tree]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[use-declarations|use declarations]]
- [[scope]]
- [[crate]]
- [[privacy]]
- [[e0603-private-item|E0603 private item]]
- [[nested-use-paths|nested use paths]]
- [[pub-use|pub use]]

## Sources

- [[7-packages-crates-and-modules-the-rust-programming-language]]
- [[7-2-control-scope-and-privacy-with-modules-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
