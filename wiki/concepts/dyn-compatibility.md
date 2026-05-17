---
title: "dyn Compatibility"
type: concept
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, traits, dyn, trait-objects]
source_count: 1
---

# dyn Compatibility

## Short Definition

dyn compatibility — traitdan `dyn Trait` trait object yasash mumkinmi, yo'qmi degan qoida to'plami.

## Why It Matters

Har traitdan `&dyn Trait` yoki `Box<dyn Trait>` qilib bo'lmaydi. Trait object ishlatish mumkin bo'lgan va bo'lmaydigan holatlarni ajratish runtime polymorphismni to'g'ri tanlash uchun kerak.

## Mental Model

Trait object uniform vtable bilan ishlashi kerak. Agar method concrete implementing type haqida juda ko'p ma'lumot talab qilsa, compiler umumiy trait object interface yasay olmaydi.

Ko'p uchraydigan to'siqlar:

- method `Self`ni receiverdan tashqari argument yoki returnda ishlatadi
- static method receiver olmaydi
- method yangi generic parameter e'lon qiladi
- trait object ishlashi uchun kerakli associated itemlar concrete holatda aniqlanmagan

Beginner source buni soddalashtirib beradi; amaliy qoida shuki, `dyn` ishlatmoqchi bo'lsangiz trait object orqali method chaqiruvi bitta umumiy interfacega tushadimi yo'qmi deb o'ylang.

## Syntax and Examples

dyn-compatible trait:

```rust
trait CanIntroduce {
    fn introduce(&self) -> String;
}
```

dyn-compatible emas:

```rust
trait Factory {
    fn make_default() -> Self;
}
```

Generic method ham to'siq bo'lishi mumkin:

```rust
trait Parser {
    fn parse<T>(&self) -> T;
}
```

## Common Mistakes

- Associated type bor har qanday trait dyn-compatible emas deb o'ylash. Bu haddan tashqari soddalashtirish; ba'zi traitlar associated type concrete ko'rsatilsa trait object bo'la oladi.
- `dyn Trait` ishlamasa, muammo `Sized`dadir deb o'ylab qolish. Ko'pincha asl sabab trait method signaturesida bo'ladi.
- `Self` faqat return positionda muammo beradi deb o'ylash; receiverdan tashqari argumentlarda ham muammo bo'lishi mumkin.

## Related Concepts

- [[trait-object|trait object]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[sized-trait|Sized trait]]
- [[traits]]
- [[self-type|Self type]]

## Sources

- [[wiki/sources/rust-for-backend-developers-traits]]
