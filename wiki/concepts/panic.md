---
title: "panic"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, errors]
source_count: 7
---

# panic

## Short Definition

`panic` yoki `panic!` Rust program recover qilinmaydigan xato holatida bajarishni to'xtatadigan runtime failure.

## Why It Matters

Rust ba'zi invalid operationsni memory unsafetyga aylantirmaydi; masalan out-of-bounds indexing panic qiladi. Panic bugni ko'rinadigan qiladi, lekin recoverable holatlar uchun [[option|Option]] yoki [[result|Result<T, E>]] bilan explicit handling ko'pincha yaxshiroq.

## Mental Model

Panic "bu pathda program davom eta olmaydi" degani. Beginner code'da `.expect(...)`, invalid array/vector indexing, yoki explicit `panic!` panicga olib kelishi mumkin.

Default response [[stack-unwinding|stack unwinding]]: Rust stack bo'ylab ortga yurib cleanup qiladi va programni tugatadi. [[abort-on-panic|Abort on panic]] esa cleanup walk qilmasdan programni darhol tugatadi.

`unwrap` va `expect` ham `Err` yoki `None` holatda panic qiladi. `expect` message orqali assumptionni yaxshiroq tushuntiradi.

9.3dan keyingi rule: panic bad state, broken invariant, broken contract, yoki continuing harmful/security risk bo'lgan holatlar uchun mos. Expected failures uchun `Result` yaxshiroq default.

## Syntax and Examples

Explicit panic:

```rust
fn main() {
    panic!("crash and burn");
}
```

Out-of-bounds panic:

```rust
let v = vec![1, 2, 3];
let value = &v[100]; // panic
```

Recoverable alternative:

```rust
match v.get(100) {
    Some(value) => println!("{value}"),
    None => println!("No value at that index"),
}
```

Backtrace bilan tekshirish:

```shell
$ RUST_BACKTRACE=1 cargo run
```

Contract violation example:

```rust
pub fn new(value: i32) -> Guess {
    if value < 1 || value > 100 {
        panic!("Guess value must be between 1 and 100, got {value}.");
    }

    Guess { value }
}
```

## Common Mistakes

- User input kabi normal xatolar uchun panicni default qilish.
- Panic memory unsafety degani deb o'ylash; Rust panic orqali invalid memory accessni oldini olishi mumkin.
- `.unwrap()`/`.expect()`ni real error handling o'rniga odat qilish.
- Panic outputidagi library locationni root cause deb qabul qilib, [[backtrace]]dan o'z source line'ingizni qidirmaslik.
- `unwrap`/`expect`ni recoverable error handling o'rniga qo'yish.
- Expected failure bilan broken invariantni aralashtirish.
- Public API panic shartlarini documentationda yozmaslik.

## Related Concepts

- [[option|Option]]
- [[result|Result]]
- [[vector-indexing|vector indexing]]
- [[array]]
- [[error-handling]]
- [[unrecoverable-errors|unrecoverable errors]]
- [[stack-unwinding|stack unwinding]]
- [[abort-on-panic|abort on panic]]
- [[backtrace]]
- [[buffer-overread|buffer overread]]
- [[unwrap]]
- [[expect]]
- [[panic-vs-result|panic! vs Result]]
- [[bad-state|bad state]]
- [[invariants]]
- [[function-contracts|function contracts]]

## Sources

- [[0-2-introduction-the-rust-programming-language]]
- [[3-2-data-types-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[9-error-handling-the-rust-programming-language]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
- [[9-2-recoverable-errors-with-result-the-rust-programming-language]]
- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
