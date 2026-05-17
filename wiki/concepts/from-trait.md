---
title: "From Trait"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits, conversion, error-handling]
source_count: 1
---

# From Trait

## Short Definition

`From` trait bir type'dan boshqa type'ga conversionni belgilaydi. `?` operator error propagation paytida error type conversion uchun `From::from`dan foydalanishi mumkin.

## Why It Matters

Function bitta umumiy error type qaytarishi mumkin, lekin ichida turli operationlar turli error typelar qaytaradi. `From` implementationlari `?` operatorga bu errorlarni current function return error type'iga convert qilishga imkon beradi.

## Mental Model

`?` errorni qaytarayotganda "bu error current function error type'iga qanday aylanadi?" deb so'raydi. Javob `From` implementationida bo'lishi mumkin.

## Syntax and Examples

Conceptual example:

```rust
impl From<std::io::Error> for OurError {
    fn from(error: std::io::Error) -> OurError {
        OurError::Io(error)
    }
}
```

Shundan keyin `Result<_, std::io::Error>` ustidagi `?` `OurError` qaytaradigan function ichida ishlashi mumkin.

## Common Mistakes

- `?` hamma error typelarni avtomatik convert qiladi deb o'ylash.
- Conversion uchun `From` yoki boshqa explicit mapping kerakligini unutish.
- `From`ni generic trait bounds mavzusi bilan bog'lamaslik.

## Related Concepts

- [[traits]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[result|Result<T, E>]]

## Sources

- [[9-2-recoverable-errors-with-result]]
