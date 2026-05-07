---
title: "Cargo.toml"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, cargo]
source_count: 6
---

# Cargo.toml

## Short Definition

`Cargo.toml` Cargo package manifest file bo'lib, package metadata, build configuration, va dependenciesni saqlaydi.

## Common Commands

```shell
$ cargo new hello_cargo
$ cargo add rand@0.8.5
```

## Notes

- `[package]` ichida `name`, `version`, va `edition` kabi metadata bo'ladi.
- `[dependencies]` external crates ro'yxatini belgilaydi.
- [[cargo-lock|Cargo.lock]] exact resolved versionsni yozib boradi.
- `src/main.rs` yoki `src/lib.rs` odatda `Cargo.toml` ichida alohida yozilmaydi; Cargo convention orqali ularni crate root deb biladi.
- `Cargo.toml` [[package]]ni describe qiladi; [[crate-root|crate root]] esa source file'dan boshlanadi.
- External crate ishlatishda `Cargo.toml` dependency'ni beradi, lekin code ichida kerakli item/trait [[use-declarations|use]] bilan scope'ga olib kirilishi mumkin.
- `[profile.release] panic = 'abort'` [[release-build|release build]] paytida [[panic|panic!]] bo'lsa [[stack-unwinding|stack unwinding]] o'rniga [[abort-on-panic|abort on panic]] strategy'ni tanlaydi.

```toml
[profile.release]
panic = 'abort'
```

## Related Pages

- [[cargo|Cargo]]
- [[package]]
- [[crate]]
- [[crate-root|crate root]]
- [[dependencies]]
- [[use-declarations|use declarations]]
- [[rust-2024-edition|Rust 2024 Edition]]
- [[release-build|release build]]
- [[abort-on-panic|abort on panic]]

## Sources

- [[0-the-rust-programming-language-the-rust-programming-language]]
- [[1-3-hello-cargo-the-rust-programming-language]]
- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[7-1-packages-and-crates-the-rust-programming-language]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
