---
title: "Cargo"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, tool, cargo]
source_count: 7
---

# Cargo

## Short Definition

Cargo Rustning build system va package manageri. U project yaratish, build qilish, run qilish, dependencylarni boshqarish, va release build tayyorlash workflowini standartlashtiradi.

## Common Commands

```shell
$ cargo --version
$ cargo new hello_cargo
$ cargo init
$ cargo build
$ cargo run
$ cargo check
$ cargo add rand@0.8.5
$ cargo update
$ cargo doc --open
$ cargo build --release
$ cargo install ripgrep       # binary tool o'rnatish
$ cargo uninstall ripgrep     # o'chirish
$ cargo publish               # crates.io'ga publish
$ cargo publish -p crate_name # workspace'da aniq crate
$ cargo run -p crate_name     # workspace'da aniq crate
$ cargo test -p crate_name    # workspace'da aniq crate test
$ cargo --list                # barcha subcommandlar (custom ham)
```

## Project Layout

```text
hello_cargo/
  Cargo.toml
  Cargo.lock
  src/
    main.rs
  target/
```

Binary va library crates bo'lgan package:

```text
my-project/
  Cargo.toml
  src/
    main.rs    # binary crate root
    lib.rs     # library crate root
    bin/
      tool.rs  # additional binary crate
```

## Notes

- `cargo new` yangi project yaratadi va default holatda Git repository ham initialize qiladi.
- `cargo new my-project` `Cargo.toml` va `src/main.rs` yaratib, binary package hosil qiladi.
- `Cargo.toml` project configuration file; u TOML formatida yoziladi.
- `Cargo.toml` package manifesti bo'lib, Cargo'ga package crates qanday build qilinishini bildiradi.
- `[package]` project metadata: `name`, `version`, `edition`.
- `[dependencies]` project ishlatadigan crates ro'yxati.
- `Cargo.lock` dependencylarning exact versionsini saqlaydi; uni Cargo boshqaradi.
- `src/main.rs` Cargo convention bo'yicha default binary crate root.
- `src/lib.rs` mavjud bo'lsa, default library crate root.
- `src/bin/*.rs` ichidagi har bir file alohida binary crate.
- Cargo crate root file'larni `rustc`ga uzatadi.
- `cargo check` executable yaratmaydi, lekin code compile bo'lishini tez tekshiradi.
- `cargo add` dependency qo'shadi.
- `cargo update` `Cargo.lock`dagi versionsni `Cargo.toml` constraints doirasida yangilaydi.
- `cargo doc --open` project va dependency documentationni local browserda ochadi.
- `cargo build --release` optimized binaryni `target/release/` ichiga yaratadi.

## Workspace

```toml
# workspace root Cargo.toml
[workspace]
resolver = "3"
members = ["adder", "add_one"]
```

Workspace'da bitta `Cargo.lock` va bitta `target/` — barcha crate'lar uchun umumiy. Har crate o'z `Cargo.toml`'iga ega.

## cargo install

`cargo install` binary target bo'lgan crate'larni `~/.cargo/bin/` ga o'rnatadi. `$PATH`'da `~/.cargo/bin` bo'lishi kerak.

## Custom subcommands

`$PATH`'da `cargo-something` binary bo'lsa → `cargo something` sifatida ishlatiladi. `cargo --list` barcha subcommandlarni ko'rsatadi.

## Related Pages

- [[1-3-hello-cargo-the-rust-programming-language]]
- [[2-programming-a-guessing-game-the-rust-programming-language]]
- [[1-getting-started|1. Getting Started]]
- [[cargo-toml|Cargo.toml]]
- [[cargo-workspaces|cargo workspaces]]
- [[crates-io|crates.io]]
- [[release-build|release build & profiles]]
- [[crate]]
- [[package]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[crate-root|crate root]]
- [[dependencies]]
- [[rand]]
