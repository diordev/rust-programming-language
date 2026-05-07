---
title: "process::exit"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, process, exit, cli, error-handling]
source_count: 1
---

# process::exit

## Short Definition

`std::process::exit(code: i32)` — dasturni darhol to'xtatadi va berilgan exit kodini operatsion tizimga qaytaradi. `0` — muvaffaqiyat; boshqa qiymatlar — xato.

## Why It Matters

`panic!` dasturni to'xtatadi, lekin foydalanuvchiga "thread 'main' panicked at..." kabi texnik xabar chiqaradi. CLI dasturlarda bu yoqimsiz. `process::exit(1)` esa faqat oldindan chiqarilgan xabarni ko'rsatib, toza to'xtatadi.

## Mental Model

```
panic!("...")     → texnik stack trace, RUST_BACKTRACE xabari
process::exit(1)  → hech qanday qo'shimchasiz darhol chiqish
```

## Syntax and Examples

```rust
use std::process;

fn main() {
    let config = Config::build(&args).unwrap_or_else(|err| {
        println!("Problem parsing arguments: {err}");
        process::exit(1);  // xato holat: exit code 1
    });

    if let Err(e) = run(config) {
        println!("Application error: {e}");
        process::exit(1);
    }
}
```

### Exit kodlari konventsiyasi:

| Kod | Ma'nosi |
|---|---|
| `0` | Muvaffaqiyat |
| `1` | Umumiy xato |
| `2` | Misuse (shell konventsiya) |
| boshqalar | Dasturga xos |

### `process::exit` va `panic!` farqi:

- `panic!` — dasturchi xatosi (bug), foydalanuvchi emas
- `process::exit` — usage xatosi yoki kutilgan xato holati

## Common Mistakes

- `process::exit(0)` — xato bo'lsa ham 0 bilan chiqish — shell scriptlarda xato sezilmaydi
- `panic!` ni foydalanuvchi xatosi uchun ishlatish

## Related Concepts

- [[panic|panic!]]
- [[unwrap-or-else|unwrap_or_else]]
- [[error-handling|error handling]]
- [[separation-of-concerns|separation of concerns]]
- [[stderr|stderr]]

## Sources

- [[12-3-refactoring-to-improve-modularity-and-error-handling-the-rust-programming-language]]
