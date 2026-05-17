---
title: "Module File Layout"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, modules, file-layout]
source_count: 2
---

# Module File Layout

## Short Definition

Module file layout `mod` declarations va filesystem file'lari Rust [[module-tree|module tree]]ga qanday ulanayotganini belgilaydigan convention.

## Why It Matters

Module code kattalashganda uni alohida file'larga ajratish navigationni yengillashtiradi. Shu bilan birga module tree va public API o'zgarmasdan qolishi mumkin.

## Mental Model

`mod front_of_house;` compilerga module body'ni qayerdan topishni aytadi. File topilgandan keyin boshqa code shu module'ga file path bilan emas, module path bilan murojaat qiladi.

`mod` file'ni crate'ga ulaydi; [[use-declarations|use]] esa faqat scope ichida path shortcut yaratadi. Beginner backend source inline module, alohida `name.rs`, va directory + `mod.rs` patternini bitta mental modelga jamlaydi; `mod.rs` supported, lekin yagona layout emas.

## Syntax and Examples

```rust
// src/lib.rs
mod front_of_house;
```

Compiler modern style bo'yicha `src/front_of_house.rs`ni qidiradi.

```rust
// src/front_of_house.rs
pub mod hosting;
```

Compiler child module uchun `src/front_of_house/hosting.rs`ni qidiradi.

```text
src/
  lib.rs
  front_of_house.rs
  front_of_house/
    hosting.rs
```

Older supported style:

```text
src/
  lib.rs
  front_of_house/
    mod.rs
    hosting/
      mod.rs
```

## Common Mistakes

- `mod`ni textual include deb o'ylash.
- `use` declaration file'ni compile qiladi deb o'ylash.
- Child module file'ini parent module directory'si ostiga qo'ymaslik.
- Bitta module uchun `name.rs` va `name/mod.rs` style'larini birga ishlatish.

## Related Concepts

- [[module]]
- [[module-tree|module tree]]
- [[crate-root|crate root]]
- [[mod-declarations|mod declarations]]
- [[use-declarations|use declarations]]
- [[paths]]
- [[pub-use|pub use]]

## Sources

- [[7-5-separating-modules-into-different-files]]
- [[wiki/sources/rust-for-backend-developers-modules]]
