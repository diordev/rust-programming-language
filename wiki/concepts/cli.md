---
title: "CLI (Command Line Interface)"
type: concept
status: draft
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cli, tools]
source_count: 1
---

# CLI (Command Line Interface)

## Short Definition

CLI dasturi — terminal orqali ishlatiladigan, argumentlar va flaglar orqali boshqariladigan dastur. Rust CLI dasturlari uchun kuchli til: tez kompilyatsiya, yagona binary, cross-platform.

## Why It Matters

Ko'plab Rust asboblari CLI: `cargo`, `rustfmt`, `clippy`, `ripgrep`. CLI qurishni o'rganish Rust'ning real-world qo'llanishini ko'rsatadi.

## Mental Model

CLI dasturi: `stdin` + argumentlar → mantiq → `stdout` / `stderr`.

- `stdout` — muvaffaqiyatli natija
- `stderr` — xato xabarlari
- `exit code` — 0 muvaffaqiyat, boshqasi xato

## Syntax and Examples

Oddiy CLI argumentlarni o'qish:

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    // args[0] — binary nomi
    // args[1..] — foydalanuvchi argumentlari
}
```

## Common Mistakes

- `args[1]` mavjudligini tekshirmay indeksga murojaat — `index out of bounds` panici.
- Xato xabarlarini `println!` bilan `stdout`ga chiqarish — `eprintln!` va `stderr` ishlatish kerak.

## Related Concepts

- [[env-args|std::env::args]]
- [[stderr]]
- [[stdout]]
- [[process-exit|process::exit]]
- [[eprintln-macro|eprintln!]]

## Sources

- [[12-1-accepting-command-line-arguments-the-rust-programming-language]]
- [[12-an-io-project-the-rust-programming-language]]
