---
title: "Scope"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, ownership, modules]
source_count: 6
---

# Scope

## Short Definition

Scope item yoki name valid bo'lgan nested code context. Variable declarationdan keyin valid bo'ladi va scope tugaganda invalid bo'ladi.

## Why It Matters

Ownershipda value owner scope'dan chiqqanda dropped bo'ladi. [[module-system|Module system]]da esa scope qaysi nomlar ko'rinishini va ma'lum joydagi nom qaysi item'ga tegishli ekanini belgilaydi.

## Mental Model

```rust
{
    let s = String::from("hello");
    // s is valid here
} // s is no longer valid here
```

## Syntax and Examples

Curly braces yangi inner scope yaratadi:

```rust
fn main() {
    {
        let s = "hello";
    }
}
```

Module system kontekstida scope ichida names bo'ladi: variable, function, struct, enum, module, constant, va boshqa items.

`use` declaration ham scope bilan bog'liq: path current scope'ga shortcut sifatida olib kiriladi.

```rust
use crate::garden::vegetables::Asparagus;
```

Shundan keyin shu scope ichida `Asparagus` qisqa nomi ishlaydi.

`use` child module'ga avtomatik o'tmaydi:

```rust
use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        // hosting bu scope ichida import qilinmagan
    }
}
```

Block scope expression sifatida ham ishlaydi:

```rust
let sum = {
    let x = 1;
    let y = 2;
    x + y
};
```

Oxirgi expressiondan keyin `;` qo'yilsa, block natijasi `()` bo'lib qoladi.

`if` branches ham braces bilan yozilgani uchun aynan shu block qoidalariga bo'ysunadi.

## Common Mistakes

- Variable declarationdan oldin valid deb o'ylash.
- Inner scope tugagandan keyin inner variable ishlatishga urinish.
- Heap data cleanup'i scope tugaganda bo'lishini unutish.
- Bitta scope ichida bir xil nomli ikki item bo'lishi mumkin deb o'ylash.
- `use` declaration qaysi scope'da amal qilishini unutish.
- Parent module'dagi `use` child module ichida ham bor deb kutish.

## Related Concepts

- [[ownership]]
- [[drop]]
- [[variables-and-mutability|variables and mutability]]
- [[module-system|module system]]
- [[module]]
- [[paths]]
- [[use-declarations|use declarations]]
- [[privacy]]
- [[use-aliases|use aliases]]
- [[statements-and-expressions|statements and expressions]]
- [[unit-type|unit type]]

## Sources

- [[4-1-what-is-ownership]]
- [[wiki/sources/7-packages-crates-and-modules]]
- [[7-2-control-scope-and-privacy-with-modules]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword]]
- [[wiki/sources/rust-for-backend-developers-scopes]]
- [[wiki/sources/rust-for-backend-developers-if]]
