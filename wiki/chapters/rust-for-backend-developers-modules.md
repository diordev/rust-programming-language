---
title: "Rust for Backend Developers: Modules"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, modules, chapter]
source_count: 1
---

# Rust for Backend Developers: Modules

## Learning Goal

`mod`, `pub`, `use`, module file layout, va `crate` mental modelini chalkashtirmasdan tushunish.

## Main Ideas

- Modules name conflictni hal qiladi va codeni mantiqiy bo'ladi.
- Items private by default; `pub` bilan external access ochiladi.
- `mod` module code'ini crate tree'ga ulaydi, `use` esa path shortcut yaratadi.
- Inline module, alohida `*.rs` file, va directory + `mod.rs` patternlari bor.
- Compiler crate rootdan boshlaydi va module tree'ni shu crate compile unit'i sifatida ko'radi.

## Concepts

- [[module-system|module system]]
- [[module]]
- [[pub-keyword|pub keyword]]
- [[use-declarations|use declarations]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]
- [[crate]]
- [[crate-root|crate root]]

## Examples

```rust
mod a {
    pub fn get_num() -> i32 { 1 }
}
```

```rust
mod my_module;
use my_module::get_num;
```

## Exercises

- `src/math.rs` va `src/math/add.rs` bilan bitta kichik module tree yarating.
- `pub`ni notog'ri joyga qo'yib, qaysi item private qolishini tekshiring.

## Review Questions

- `mod` va `use` orasidagi farq nima?
- Nega `pub mod` child itemlarni avtomatik public qilmaydi?
- `crate root` module tree uchun nimani belgilaydi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-modules]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[module-system|module system]]
- [[crate]]
