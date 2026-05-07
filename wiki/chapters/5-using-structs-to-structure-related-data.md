---
title: "5. Using Structs to Structure Related Data"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, structs]
source_count: 4
---

# 5. Using Structs to Structure Related Data

## Learning Goal

Rustda related data'ni domain-specific type sifatida model qilish: [[structs]], fields, instances, shorthand syntax, update syntax, tuple structs, unit-like structs, ownership of struct data, debugging derived traits, va [[methods]] orqali behaviorni type bilan bog'lash.

## Main Ideas

- Struct custom data type bo'lib, related valuesni meaningful group qiladi.
- Structs [[tuple]]ga qaraganda readableroq, chunki values field names orqali nomlanadi.
- Struct definition type template, struct instance esa concrete value.
- Field access dot notation bilan bo'ladi.
- Mutation butun instance darajasida: individual fields alohida `mut` qilinmaydi.
- [[field-init-shorthand]] field va variable names bir xil bo'lsa repetitionni kamaytiradi.
- [[struct-update-syntax]] boshqa instance valuesidan foydalanadi va [[ownership]]/move rulesga bo'ysunadi.
- [[tuple-structs]] tuple layoutga o'xshaydi, lekin distinct named type yaratadi.
- [[unit-like-structs]] datasiz type bo'lib, keyin [[traits]] implementation uchun foydali bo'lishi mumkin.
- Struct fields owned `String` bo'lsa, instance o'z data'siga own qiladi; reference fields uchun [[lifetimes]] kerak.
- [[rectangle-example|Rectangle example]] separate variables -> [[tuple]] -> [[structs|struct]] -> [[methods|method]] refactoring yo'lini ko'rsatadi.
- Borrowed struct parameter `&Rectangle` ownershipni olmaydi va fieldlarni move qilmasdan o'qishga imkon beradi.
- Custom structni debug qilish uchun [[derive-attribute|derive attribute]] bilan [[debug-trait|Debug trait]] qo'shiladi.
- [[display-formatting|Display formatting]] user-facing, [[debug-formatting|Debug formatting]] developer-facing output uchun.
- [[dbg-macro|dbg! macro]] expressionni chiqaradi, ownershipni qaytaradi, va [[stderr]]ga yozadi.
- [[impl-block|impl block]] type bilan associated behaviorni saqlaydi.
- [[self-parameter|self parameter]] method receiver ownership/borrowing semanticsini bildiradi: `self`, `&self`, yoki `&mut self`.
- Rust method callsda [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]] ishlatadi; C/C++dagi `->` operator yo'q.
- [[associated-functions]] `self` olmasa method emas; `Rectangle::square(3)` kabi constructor-like helper bo'lishi mumkin.
- [[multiple-impl-blocks|Multiple impl blocks]] valid syntax.

## Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[struct-update-syntax]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[domain-modeling|domain modeling]]
- [[ownership]]
- [[lifetimes]]
- [[rectangle-example|rectangle area example]]
- [[debug-trait|Debug trait]]
- [[derive-attribute|derive attribute]]
- [[debug-formatting|Debug formatting]]
- [[dbg-macro|dbg! macro]]
- [[impl-block|impl block]]
- [[methods]]
- [[method-syntax|method syntax]]
- [[self-parameter|self parameter]]
- [[associated-functions]]

## Examples

Basic struct:

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Field init shorthand:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

Struct update syntax:

```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1
};
```

Tuple struct:

```rust
struct Point(i32, i32, i32);
let origin = Point(0, 0, 0);
```

Unit-like struct:

```rust
struct AlwaysEqual;
let subject = AlwaysEqual;
```

Debug derive:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

println!("rect1 is {rect1:?}");
```

Method:

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

Associated function:

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

## Exercises

- Tuple bilan represented user data'ni named `User` structga aylantirish.
- `User` instance yaratib, field access va field update mashq qilish.
- `build_user` functionini shorthand bilan va shorthand'siz yozib solishtirish.
- Struct update syntaxdan keyin oldingi instance qaysi holatda valid qolishini tekshirish.
- Tuple struct va regular tuple type safety farqini kichik function bilan sinash.
- Struct ichida `&str` fieldlar yozib [[e0106-missing-lifetime-specifier|E0106]]ni kuzatish.
- Rectangle area programini separate variablesdan structgacha refactor qilish.
- `#[derive(Debug)]`siz `println!("{rect1:?}")` qilib [[e0277-trait-bound-not-satisfied|E0277]]ni ko'rish.
- `dbg!(rect1)` va `dbg!(&rect1)` ownership farqini tekshirish.
- `area(&Rectangle)` free functionni `area(&self)` methodga aylantirish.
- `can_hold(&self, other: &Rectangle)` methodini yozish.
- `Rectangle::square(size)` associated function yaratish.

## Review Questions

- Struct tupledan qaysi jihatlar bilan farq qiladi?
- Struct field orderi qachon muhim emas?
- Nima uchun Rust individual struct fieldlarni alohida `mut` qilishga ruxsat bermaydi?
- Field init shorthand qachon ishlaydi?
- `..user1` syntaxida ownership qanday ko'chadi?
- Tuple struct qanday qilib same-field tuplelardan ham distinct type beradi?
- Unit-like struct qachon foydali bo'lishi mumkin?
- Struct reference fields saqlasa, nima uchun lifetimes kerak?
- `Display` va `Debug` formatting farqi nima?
- `dbg!` nima uchun ownership bilan bog'liq?
- Method qachon free functiondan yaxshiroq?
- `&self` va `&mut self` tanlovi nimani bildiradi?
- Associated function va method orasidagi farq nima?
- Nima uchun Rustda method call uchun `->` operator yo'q?

## Related Pages

- [[5-using-structs-to-structure-related-data-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
- [[5-3-methods-the-rust-programming-language]]
- [[4-understanding-ownership|4. Understanding Ownership]]
- [[tuple]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]
- [[rust-learning-roadmap]]
