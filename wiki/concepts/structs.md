---
title: "Structs"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, structs, types]
source_count: 6
---

# Structs

## Short Definition

`struct` related valuesni named fields bilan bitta custom data type qilib group qiladi.

## Why It Matters

Structs Rustda domain data'ni primitive values yoki tuples o'rniga meaningful type sifatida ifodalashga yordam beradi. Bu code readability va [[type-checking|type checking]]ni kuchaytiradi.

## Mental Model

Struct definition type template. [[struct-instances|Struct instance]] esa shu templatedagi har bir field uchun concrete value beradi. Field names data meaningni orderdan mustaqil qiladi.

Structs bilan ishlash progression: separate variables yoki [[tuple]] o'rniga named fields ishlatiladi, debugging uchun [[debug-trait|Debug trait]] derive qilinadi, behavior esa [[impl-block|impl block]] ichidagi [[methods]] orqali type bilan bog'lanadi.

Module privacy kontekstida `pub struct` type'ni public qiladi, lekin fields private by default qoladi. Public fields API contract bo'lib qoladi; private fields uchun public associated function constructor vazifasini bajarishi mumkin.

Generic structs field typelarini [[generic-type-parameters|generic type parameters]] bilan abstract qiladi. `Point<T>`da `x` va `y` bir xil concrete type bo'ladi; `Point<T, U>`da esa `x` va `y` turli typelar bo'lishi mumkin.

## Syntax and Examples

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

let user1 = User {
    active: true,
    username: String::from("someusername123"),
    email: String::from("someone@example.com"),
    sign_in_count: 1,
};
```

Generic struct:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

let point = Point { x: 5, y: 4.0 };
```

## Common Mistakes

- Structni faqat tuple'ning uzunroq syntax'i deb o'ylash.
- Struct instance mutable bo'lmasa field assignment ham mumkin deb o'ylash.
- Struct fields reference bo'lsa [[lifetimes]] kerakligini unutish.
- Struct behaviorini doim free functions sifatida qoldirish; method organization ko'pincha aniqroq.
- Custom struct avtomatik `Display` yoki `Debug` formatlanadi deb o'ylash.
- `pub struct` fieldsni ham avtomatik public qiladi deb o'ylash.
- `Point<T>` kabi bitta generic parameter ishlatilgan structda fields turli type bo'lishi mumkin deb o'ylash.

## Related Concepts

- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[struct-update-syntax]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[domain-modeling|domain modeling]]
- [[ownership]]
- [[methods]]
- [[impl-block|impl block]]
- [[debug-trait|Debug trait]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[generics]]
- [[generic-structs|generic structs]]
- [[generic-type-parameters|generic type parameters]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Sources

- [[5-using-structs-to-structure-related-data-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
- [[5-3-methods-the-rust-programming-language]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
