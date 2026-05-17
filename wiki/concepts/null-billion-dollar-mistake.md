---
title: "Null va Billion Dollar Mistake"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, option, design, history]
source_count: 1
---

# Null va Billion Dollar Mistake

## Short Definition

Tony Hoare 2009 yilda "null reference"ni o'zining "billion dollar mistake"i deb atagan. Rust null'ni qo'shmagan; o'rniga "value bor / yo'q"ni [[option|Option<T>]] enumi orqali type system'da ifodalaydi.

## Why It Matters

Null ko'p dasturlash tilida runtime crash va vulnerability manbai. Rust dizayn pog'onasida shu xato'ni qaytarmaslikka qaror qilgan: agar value yo'q bo'lishi *mumkin* bo'lsa, uning type'i `Option<T>` bo'lishi shart. Boshqa joyda value type `Option<T>` bo'lmasa, u valid ekaniga ishonch bilan tayanish mumkin.

## Mental Model

Null'ning muammosi konseptda emas, *implementation*da: har qanday referenceni implicitly null'ga aylantirish "ushbu value'ni ishlatishdan oldin tekshirish kerak" qoidasini hamma joyga tarqatadi. Rust konseptni saqlab, lekin uni *opt-in* qiladi:

- `T` — har doim valid value bor.
- `Option<T>` — value yo'q bo'lishi mumkin, *handle qilish shart*.

Compiler ikkalasi orasidagi farqni majburlay oladi va eng ko'p uchraydigan null-related xato — "bu null emas deb taxmin qilish" — compile time'da tutiladi.

## Syntax and Examples

Hoare quote (1965 dizayn qaroriga oid):

> I call it my billion-dollar mistake. ... I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement.

Java/C# style "implicit null":

```text
String name = null;       // har qanday reference null bo'la oladi
name.length();            // runtime NullPointerException
```

Rust style "explicit absence":

```rust
let name: Option<String> = None;

// Agar `String` lazim bo'lsa, type `String` deb beriladi va u null bo'lolmaydi.
let surely_a_name: String = String::from("Ada");
```

Compile-time checking:

```rust
let n: Option<i32> = Some(5);
let v: i32 = n;                  // compile error: type mismatch
let v: i32 = n.unwrap_or(0);     // explicit conversion
```

## Common Mistakes

- "Rustda null yo'q" — texnik jihatdan to'g'ri, lekin `Option::None` xuddi shu konseptni ifodalaydi (shu sabab "value yo'q" holatini hisobga olish kerak).
- `.unwrap()` ko'p ishlatib null-style panic'larni qaytarib olib kelish.
- `Option<&T>` borrow chainida `Option<T>`ga aylantirishni bilmaslik (`as_ref`, `as_deref`).

## Related Concepts

- [[option|Option]]
- [[enums]]
- [[pattern-matching|pattern matching]]
- [[match]]
- [[memory-safety|memory safety]]

## Sources

- [[6-1-defining-an-enum]]
