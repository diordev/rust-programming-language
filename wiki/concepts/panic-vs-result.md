---
title: "panic! vs Result"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, error-handling, panic, result]
source_count: 2
---

# panic! vs Result

## Short Definition

`panic!` unrecoverable yoki contract-breaking holatni bildiradi; `Result<T, E>` esa caller recover qila oladigan failure'ni bildiradi.

## Why It Matters

Bu tanlov API semantics'ini belgilaydi.

## Mental Model

Default rule: caller nimadir qila olsa, `Result` qaytaring. Panicni bug, broken invariant, impossible state, yoki harmful continuation uchun saqlang.

Advance source yana bitta qat'iy chiziq chizadi: `catch_unwind` va `panic_any` mavjud bo'lsa ham ular `Result`ni almashtirmaydi.

## Syntax and Examples

```rust
fn parse_port(input: &str) -> Result<u16, std::num::ParseIntError> {
    input.parse()
}
```

```rust
fn force_contract(v: i32) {
    if v < 0 {
        panic!("negative is invalid here");
    }
}
```

## Common Mistakes

- Request yoki parser xatolarini panic qilish.
- Public library API'da callerga recover imkonini bermaslik.
- Panic boundary isolationni normal error handling deb o'qish.

## Related Concepts

- [[panic]]
- [[result|Result<T, E>]]
- [[catch-unwind]]
- [[panic-any]]
- [[invariants]]
- [[function-contracts|function contracts]]

## Sources

- [[9-3-to-panic-or-not-to-panic]]
- [[wiki/sources/rust-for-backend-developers-panic]]

