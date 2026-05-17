---
title: "Boxed Closure Handlers"
type: example
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, closures, trait-objects, impl-trait]
source_count: 1
---

# Boxed Closure Handlers

## Context

Rust Book Listings 20-32, 20-33, va 20-34 closure qaytarishda `impl Fn` va `Box<dyn Fn>` orasidagi farqni ko'rsatadi.

## Code

Bitta closure qaytarish:

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

Muammo: ikki function bir xil `impl Fn(i32) -> i32` yozsa ham, return typelari bir xil emas:

```rust
fn main() {
    let handlers = vec![returns_closure(), returns_initialized_closure(123)];
    for handler in handlers {
        let output = handler(5);
        println!("{output}");
    }
}

fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}

fn returns_initialized_closure(init: i32) -> impl Fn(i32) -> i32 {
    move |x| x + init
}
```

Compiler [[e0308-mismatched-types|E0308]] beradi, chunki ikki `impl Fn` alohida [[opaque-types|opaque type]].

Yechim:

```rust
fn main() {
    let handlers = vec![returns_closure(), returns_initialized_closure(123)];
    for handler in handlers {
        let output = handler(5);
        println!("{output}");
    }
}

fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn returns_initialized_closure(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}
```

## Explanation

Har closure compiler yaratadigan alohida concrete typega ega. `impl Fn(i32) -> i32` concrete typeni yashiradi, lekin har return site uchun bitta maxsus type saqlanadi. Shuning uchun:

```rust
fn a() -> impl Fn(i32) -> i32 { |x| x + 1 }
fn b() -> impl Fn(i32) -> i32 { |x| x + 2 }
```

`a()` va `b()` return typelari bir xil emas.

`Box<dyn Fn(i32) -> i32>` esa umumiy concrete container type beradi:

- `Box` - heap allocation va ownership.
- `dyn Fn(...)` - trait object, runtime dispatch.
- `Fn(i32) -> i32` - callable contract.

Natija: turli closure'lar bitta `Vec<Box<dyn Fn(i32) -> i32>>` ichida saqlanadi.

## Tradeoff

| Yondashuv | Afzallik | Narx |
|-----------|----------|------|
| `impl Fn` | Static dispatch, allocation yo'q | Bitta concrete closure type |
| `Box<dyn Fn>` | Turli closure typelari bitta collectionda | Heap allocation + dynamic dispatch |

## Related Concepts

- [[returning-closures|returning closures]]
- [[closures]]
- [[opaque-types|opaque types]]
- [[impl-trait|impl Trait]]
- [[trait-object|trait object]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[box-t|Box<T>]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
