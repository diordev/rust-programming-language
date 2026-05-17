---
title: "Cargo"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, tool, cargo]
source_count: 17
---

# Cargo

## Short Definition

Cargo Rustning build system va package manageri. U project yaratish, build qilish, run qilish, dependencylarni boshqarish, va release build tayyorlash workflowini standartlashtiradi.

## Common Commands

```shell
$ cargo --version
$ cargo new hello_cargo
$ cargo new my_lib --lib
$ cargo init
$ cargo build
$ cargo run
$ cargo test
$ cargo tree
$ cargo check
$ cargo add rand@0.8.5
$ cargo update
$ cargo doc --open
$ cargo build --release
$ cargo build --bin import
$ cargo fmt
$ cargo fix
$ cargo clippy
$ cargo nextest run
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
- `[dev-dependencies]` test va development-only crates ro'yxati.
- `Cargo.lock` dependencylarning exact versionsini saqlaydi; uni Cargo boshqaradi.
- `src/main.rs` Cargo convention bo'yicha default binary crate root.
- `src/lib.rs` mavjud bo'lsa, default library crate root.
- `src/bin/*.rs` ichidagi har bir file alohida binary crate.
- TOML `[[bin]]` section bilan executable nomi va path'i explicit boshqariladi.
- `cargo new --lib` default library package layoutini yaratadi.
- Cargo crate root file'larni `rustc`ga uzatadi.
- `cargo check` executable yaratmaydi, lekin code compile bo'lishini tez tekshiradi.
- `cargo add` dependency qo'shadi.
- `cargo update` `Cargo.lock`dagi versionsni `Cargo.toml` constraints doirasida yangilaydi.
- `cargo tree` top-level va transitive dependency graph'ni ko'rsatadi.
- `cargo test` unit va integration tests'ni build qilib ishga tushiradi.
- `cargo doc --open` project va dependency documentationni local browserda ochadi.
- `cargo build --release` optimized binaryni `target/release/` ichiga yaratadi.
- `cargo build --bin name` faqat bitta binary target'ni build qiladi.
- `cargo fmt` project bo'ylab [[rustfmt]]ni ishlatadi va formattingni standardlashtiradi.
- `cargo fix` compiler diagnostics asosida mechanical fix'larni qo'llaydi; edition migration paytida ham foydali.
- `cargo clippy` standard compiler warninglaridan tashqari qo'shimcha lint qatlamini beradi.
- `cargo nextest run` mavjud Rust testlarini alternativ runner bilan ishlatadi.

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

`cargo new` va `cargo run` yangi Rust install yoki editor setup ishlayotganini smoke test qilish uchun ham ishlatiladi.

## Related Pages

- [[1-3-hello-cargo]]
- [[wiki/sources/2-programming-a-guessing-game]]
- [[wiki/chapters/1-getting-started|1. Getting Started]]
- [[cargo-toml|Cargo.toml]]
- [[cargo-workspaces|cargo workspaces]]
- [[crates-io|crates.io]]
- [[release-build|release build & profiles]]
- [[crate]]
- [[package]]
- [[binary-crate|binary crate]]
- [[library-crate|library crate]]
- [[src-bin|src/bin]]
- [[crate-root|crate root]]
- [[dependencies]]
- [[cargo-features]]
- [[workspace-dependencies]]
- [[dev-dependencies]]
- [[cargo-nextest]]
- [[rand]]
- [[rustfmt]]
- [[cargo-fix|cargo fix / rustfix]]
- [[clippy]]
- [[vscode|VSCode]]
- [[zed-editor|Zed]]

## Sources

- [[wiki/sources/2-programming-a-guessing-game|2. Programming a Guessing Game]]
- [[wiki/sources/14-more-about-cargo-and-crates-io|14. More About Cargo and Crates.io]]
- [[14-1-customizing-builds-with-release-profiles|14.1. Customizing Builds with Release Profiles]]
- [[14-2-publishing-a-crate-to-crates-io|14.2. Publishing a Crate to Crates.io]]
- [[14-3-cargo-workspaces|14.3. Cargo Workspaces]]
- [[14-4-installing-binaries-with-cargo-install|14.4. Installing Binaries with cargo install]]
- [[14-5-extending-cargo-with-custom-commands|14.5. Extending Cargo with Custom Commands]]
- [[wiki/sources/22-4-d-useful-development-tools|22.4. D - Useful Development Tools]]
- [[wiki/sources/22-5-e-editions|22.5. E - Editions]]
- [[wiki/sources/rust-for-backend-developers-setup-rust]]
- [[wiki/sources/rust-for-backend-developers-install-on-windows]]
- [[wiki/sources/rust-for-backend-developers-development-environment]]
- [[wiki/sources/rust-for-backend-developers-cargo]]
- [[wiki/sources/rust-for-backend-developers-dependencies]]
- [[wiki/sources/rust-for-backend-developers-multiple-binaries]]
- [[wiki/sources/rust-for-backend-developers-workspace-project]]
- [[wiki/sources/rust-for-backend-developers-testing]]
