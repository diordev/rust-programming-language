---
title: "Function Pointer with do_twice"
type: example
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, functions, closures]
source_count: 1
---

# Function Pointer with do_twice

## Context

Rust Book Listing 20-28 `fn` pointer syntaxini ko'rsatadi: oddiy function boshqa functionga argument sifatida uzatiladi.

## Code

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);

    println!("The answer is: {answer}");
}
```

Output:

```text
The answer is: 12
```

## Explanation

`do_twice` ikkita argument oladi:

- `f: fn(i32) -> i32` - `i32` olib `i32` qaytaradigan [[function-pointers|function pointer]].
- `arg: i32` - functionga beriladigan qiymat.

`do_twice(add_one, 5)` chaqirilganda:

```text
add_one(5) + add_one(5)
= 6 + 6
= 12
```

Lowercase `fn` closure traitlaridan farq qiladi. `Fn`, `FnMut`, `FnOnce` trait; `fn` esa concrete type. Function pointer uchala traitni implement qilgani uchun closure kutadigan ko'p joylarga named function uzatish mumkin.

## Variation

Ko'proq moslashuvchan variant:

```rust
fn do_twice<F>(f: F, arg: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(arg) + f(arg)
}
```

Bu named function ham, capturing closure ham qabul qiladi:

```rust
fn add_one(x: i32) -> i32 { x + 1 }
assert_eq!(do_twice(add_one, 5), 12);

let amount = 3;
assert_eq!(do_twice(|x| x + amount, 5), 16);
```

## Related Concepts

- [[function-pointers|function pointers]]
- [[functions]]
- [[closures]]
- [[fn-traits|Fn traits]]
- [[ffi|FFI]]

## Sources

- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
