---
title: "panic! vs Result"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, error-handling, panic, result]
source_count: 1
---

# panic! vs Result

## Short Definition

`panic!` program davom eta olmaydigan unrecoverable holatni bildiradi; `Result<T, E>` caller recover qilishi mumkin bo'lgan failure possibility'ni bildiradi.

## Why It Matters

`panic!` chaqirish caller nomidan "bu holat unrecoverable" deb qaror qilishdir. `Result` qaytarish esa callerga contextiga mos qaror berish imkonini beradi.

## Mental Model

Default rule: function fail bo'lishi mumkin va caller nimadir qila olsa, `Result` qaytaring. Panicni bad state, broken invariant, broken contract, yoki insecure/harmful continuation uchun saqlang.

`panic!` mos bo'lishi mumkin:

- examples va prototype code
- tests
- developer compilerdan ko'proq biladigan invariantlar
- broken contract yoki bad state
- continuing security risk tug'diradigan holatlar

`Result` mos bo'lishi mumkin:

- malformed user input
- missing file yoki external resource
- parser error
- network/HTTP rate limit
- caller turlicha recover qilishi mumkin bo'lgan API failure

## Syntax and Examples

Expected failure:

```rust
fn parse_port(input: &str) -> Result<u16, std::num::ParseIntError> {
    input.parse()
}
```

Compiler bilmaydigan hardcoded invariant:

```rust
let home: std::net::IpAddr = "127.0.0.1"
    .parse()
    .expect("Hardcoded IP address should be valid");
```

## Common Mistakes

- Normal user input errorini panic bilan tugatish.
- Public library API'da callerga recover imkonini bermaslik.
- Broken invariant holatida code'ni davom ettirish.
- `expect` message'ida assumptionni yozmaslik.

## Related Concepts

- [[panic|panic!]]
- [[result|Result<T, E>]]
- [[recoverable-errors|recoverable errors]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[bad-state|bad state]]
- [[invariants]]
- [[function-contracts|function contracts]]

## Sources

- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
