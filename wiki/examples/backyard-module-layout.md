---
title: "Backyard module layout"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, modules]
source_count: 1
---

# Backyard module layout

## Goal

[[crate-root|Crate root]], [[module]], [[paths]], [[use-declarations|use]], va [[pub-keyword|pub]] birga ishlaganda compiler module file'larini qanday topishini ko'rsatish.

## Code

```text
backyard/
  Cargo.lock
  Cargo.toml
  src/
    main.rs
    garden.rs
    garden/
      vegetables.rs
```

`src/main.rs`:

```rust
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}
```

`src/garden.rs`:

```rust
pub mod vegetables;
```

`src/garden/vegetables.rs`:

```rust
#[derive(Debug)]
pub struct Asparagus {}
```

## Explanation

`src/main.rs` [[binary-crate|binary crate]] uchun [[crate-root|crate root]]. `pub mod garden;` compilerga `garden` module'ini qo'shishni aytadi; compiler bu code'ni `src/garden.rs`dan topadi. `src/garden.rs` ichidagi `pub mod vegetables;` esa child module'ni `src/garden/vegetables.rs`dan topishga olib keladi.

`Asparagus` type'iga full path `crate::garden::vegetables::Asparagus`. `use crate::garden::vegetables::Asparagus;` shu path'ni `main.rs` scope'iga shortcut qilib olib kiradi, shuning uchun `main` ichida `Asparagus {}` deb yozish mumkin.

Har bir public access alohida ochilgan: `garden` public module, `vegetables` public module, `Asparagus` public struct. Agar shu `pub`lardan biri olib tashlansa, privacy qoidalari path orqali foydalanishni cheklaydi.

## Related Pages

- [[7-2-control-scope-and-privacy-with-modules-the-rust-programming-language]]
- [[module-system|module system]]
- [[module-tree|module tree]]
- [[module]]
- [[paths]]
- [[use-declarations|use declarations]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
