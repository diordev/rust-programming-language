---
title: "Use Declarations"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules]
source_count: 5
---

# Use Declarations

## Short Definition

`use` declaration path'ni current [[scope]]ga olib kirib, item'ni qisqaroq nom bilan ishlatishga yordam beradi.

## Why It Matters

Chapter 7 opener `modules and use`ni paths organization, scope, va privacy'sini boshqaradigan feature sifatida tanitadi. 7.2 section `use` long path'ni current scope ichida qisqa nom bilan ishlatish uchun shortcut yaratishini ko'rsatadi. 7.4 section esa idiomatic imports, aliases, re-exports, external crates, nested paths, va glob operatorni shu modelga qo'shadi.

## Mental Model

`use` path'ni yo'qotmaydi; u faqat item nomini muayyan scope'da qulayroq qiladi. `use` module tree'ni o'zgartirmaydi, file'ni compile qilmaydi, va [[privacy]] qoidalarini chetlab o'tmaydi.

Functionlar uchun odatda parent module import qilinadi, struct/enums uchun esa itemning o'zi full path bilan import qilinadi. [[pub-use|pub use]] bu shortcutni tashqi code uchun public re-exportga aylantiradi.

`HashMap` common collection bo'lsa ham prelude'da emas. Uni qisqa nom bilan ishlatish uchun `std::collections::HashMap` path'i `use` orqali scope'ga olib kiriladi.

## Syntax and Examples

```rust
use std::cmp::Ordering;
```

Shundan keyin shu scope ichida `Ordering` qisqa nomi ishlatilishi mumkin.

```rust
use std::collections::HashMap;
```

Shundan keyin shu scope ichida `HashMap::new()` deb yozish mumkin.

```rust
use crate::garden::vegetables::Asparagus;
```

Shundan keyin shu scope ichida `Asparagus {}` deb yozish mumkin.

Function parent module'i bilan:

```rust
use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

Alias:

```rust
use std::io::Result as IoResult;
```

Nested paths:

```rust
use std::{cmp::Ordering, io};
use std::io::{self, Write};
```

Re-export:

```rust
pub use crate::front_of_house::hosting;
```

## Common Mistakes

- `use` item'ni import qilish bilan ownership yoki runtime cost qo'shadi deb o'ylash.
- `use` butun project bo'ylab amal qiladi deb kutish; u scope bilan bog'liq.
- `use` privacy qoidalarini chetlab o'tadi deb o'ylash.
- `use` file'ni crate'ga qo'shadi deb o'ylash; bu [[mod-declarations|mod declarations]] vazifasi.
- Glob importni odatiy import style sifatida ishlatish.
- `HashMap` prelude'da deb o'ylab importni unutish.

## Related Concepts

- [[module-system|module system]]
- [[paths]]
- [[scope]]
- [[module]]
- [[module-tree|module tree]]
- [[crate]]
- [[privacy]]
- [[pub-use|pub use]]
- [[use-aliases|use aliases]]
- [[nested-use-paths|nested use paths]]
- [[glob-operator|glob operator]]
- [[mod-declarations|mod declarations]]
- [[hash-map|HashMap]]

## Sources

- [[7-packages-crates-and-modules-the-rust-programming-language]]
- [[7-2-control-scope-and-privacy-with-modules-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[7-5-separating-modules-into-different-files-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
