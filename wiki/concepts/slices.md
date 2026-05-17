---
title: "Slices"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, slices, borrowing]
source_count: 5
---

# Slices

## Short Definition

Slice collection ichidagi contiguous sequence'ga ownershipsiz reference.

## Why It Matters

Slice index valuesni data state'idan ajratib qo'ymaslikka yordam beradi. Compiler slice reference valid bo'lishini borrow rules orqali tekshiradi.

## Mental Model

Slice pointer + length saqlaydi. U owner emas, borrowed view.

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
```

Inclusive range ham ishlaydi:

```rust
let arr = [0, 1, 2, 3, 4];
let slice: &[i32] = &arr[2..=4];
```

String slices range indicesni bytes sifatida oladi va UTF-8 character boundarylarda bo'lishi kerak.

Shu ma'noda `&str` - text uchun maxsus slice. `String`dan `as_str()` bilan olinadigan borrowed view ham aslida slice modelining o'zi.

Chapter 10 openingda `largest(list: &[i32]) -> &i32` functioni slice parameter ishlatadi. Bu function har qanday `i32` slice'ni borrow qiladi, owned `Vec<i32>` talab qilmaydi.

## Syntax and Examples

String slice:

```rust
let s = String::from("hello world");
let hello = &s[0..5];
let whole = &s[..];
```

Array slice:

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
assert_eq!(slice, &[2, 3]);
```

Function parameter slice:

```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];
    // ...
    largest
}
```

Mutable subrange:

```rust
let mut v = vec![0, 1, 2, 3];
let slice: &mut [i32] = &mut v[1..3];
slice[0] = 9;
```

## Common Mistakes

- Byte indexni string state'i bilan syncda saqlashga urinish.
- UTF-8 character boundary bo'lmagan joydan string slice olish.
- Function parameterida `&String` ishlatib API'ni keraksiz tor qilish.
- String slice range'i grapheme cluster yoki character count bo'yicha ishlaydi deb o'ylash.

## Related Concepts

- [[string-slice|String slice]]
- [[reference]]
- [[borrowing]]
- [[utf-8|UTF-8]]
- [[function-extraction|function extraction]]
- [[largest-function-extraction|largest function extraction]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]

## Sources

- [[4-3-the-slice-type]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
- [[wiki/sources/rust-for-backend-developers-slices]]
- [[wiki/sources/rust-for-backend-developers-strings]]
