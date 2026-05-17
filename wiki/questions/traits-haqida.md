---
title: "Traitlar haqida"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, traits]
source_count: 3
---

# Traitlar haqida

## Answer

Rustda `trait` shared behavior contractidir. Oddiy qilib aytganda, trait type qanday ishni qila olishini belgilaydi.

Masalan:

- `Display` - user-facing text chiqarish
- `Debug` - developer-facing debug output
- `Copy` - assignment yoki function call paytida trivial copy bo'lish
- `Clone` - explicit copy qilish

Traitni "interface"ga o'xshatish mumkin, lekin Rustdagi trait bundan kuchliroq va compile-time bilan qattiq bog'langan.

### 1. Trait nima beradi?

Trait type uchun common behavior beradi.

```rust
trait Summary {
    fn summarize(&self) -> String;
}
```

Bu shuni aytadi: `Summary` ni implement qilgan har bir type `summarize()` methodini taqdim etishi kerak.

### 2. Traitni `impl` qilish

```rust
struct NewsArticle {
    headline: String,
    author: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{} by {}", self.headline, self.author)
    }
}
```

Bu yerda:

- `Summary` - contract
- `impl Summary for NewsArticle` - shu contractni `NewsArticle` uchun bajarish

### 3. Trait bound nima?

Generic function yoki type qaysi behaviorni qabul qilishini trait bound bilan aytamiz.

```rust
fn notify(item: &impl Summary) {
    println!("{}", item.summarize());
}
```

Yoki:

```rust
fn notify<T>(item: &T)
where
    T: Summary,
{
    println!("{}", item.summarize());
}
```

Bu compilerga juda muhim signal beradi: `item` dan `summarize()` chaqirish mumkin.

### 4. Nega kerak?

Traitlar quyidagi muammolarni hal qiladi:

- code duplicationni kamaytiradi
- generic code'ni xavfsiz qiladi
- type behaviorini explicit qiladi
- compile-time polymorphism beradi

Shu sababli Rustda traitlar juda markaziy tushuncha.

### 5. `derive` bilan bog'lanishi

Ko'p standard traitlar `#[derive(...)]` bilan avtomatik yoziladi.

```rust
#[derive(Debug, Clone, Copy)]
struct Point {
    x: i32,
    y: i32,
}
```

Bu yerda compiler tayyor trait implementation yozib beradi. Ammo hamma traitlar `derive` qilinmaydi.

### 6. Mashhur standard traitlar

- `Debug` - `{:?}` format uchun
- `Display` - `{}` format uchun
- `Copy` - trivial copy uchun
- `Clone` - explicit copy uchun
- `PartialEq` / `Eq` - taqqoslash uchun
- `Ord` / `PartialOrd` - tartiblash uchun
- `Default` - default value yaratish uchun

### 7. Trait bilan `impl block` farqi

- `trait` - nima bo'lishi kerakligini aytadi
- `impl` - uni qanday qilishni yozadi

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

Bu `impl` block, lekin trait implement emas. Trait implement bo'lsa `impl TraitName for TypeName` yoziladi.

## Evidence

- [[traits]]
- [[generics]]
- [[impl-block|impl block]]
- [[derive-attribute|derive attribute]]
- [[debug-trait|Debug trait]]
- [[display-formatting|Display formatting]]
- [[copy-trait|Copy trait]]
- [[clone]]
- [[5-3-methods]]
- [[wiki/sources/0-2-introduction]]

## Follow-up

- `Debug` va `Display`ni alohida misollar bilan taqqoslash.
- `Copy` va `Clone` farqini ownership misoli bilan ko'rish.
- `where` syntax va `impl Trait` syntaxni solishtirish.

## Related Pages

- [[traits]]
- [[generics]]
- [[impl-block|impl block]]
- [[derive-attribute|derive attribute]]
- [[debug-trait|Debug trait]]
- [[display-formatting|Display formatting]]
- [[copy-trait|Copy trait]]
- [[clone]]
