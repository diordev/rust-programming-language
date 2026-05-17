---
title: "Hello, World!"
type: example
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, example, beginner]
source_count: 2
---

# Hello, World!

## Purpose

Rustda minimal executable program qanday ko'rinishini va uni `rustc` bilan qanday compile/run qilishni ko'rsatadi.

## Code

```rust
fn main() {
    println!("Hello, world!");
}
```

## Explanation

- `fn main()` executable Rust programning entry pointi.
- `{}` function bodyni o'rab turadi.
- `println!` terminalga text chiqaradigan macro call.
- `"Hello, world!"` string argument sifatida uzatiladi.
- `;` expression tugaganini bildiradi.

## Run

```shell
$ rustc main.rs
$ ./main
Hello, world!
```

Windowsda run command:

```powershell
> .\main
Hello, world!
```

## Related Pages

- [[1-2-hello-world]]
- [[wiki/chapters/1-getting-started|Getting Started]]
- [[rustc]]
- [[main-function|main function]]
- [[println-macro|println! macro]]

## Sources

- [[1-2-hello-world]]
- [[wiki/sources/rust-for-backend-developers-first-look]]
