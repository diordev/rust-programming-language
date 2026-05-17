---
title: "Newtype Pattern"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, patterns, traits, types]
source_count: 3
---

# Newtype Pattern

## Short Definition

Mavjud tipni yagona-fieldli tuple struct'ga "o'rab olish" — `struct Wrapper(Vec<String>)`. Tashqaridan yangi alohida tip; ichkarida orginal tip turadi. Compile time'da o'rashlash o'chiriladi (zero overhead).

## Why It Matters

Bir nechta muhim foydalanish:

1. **[[orphan-rule|Orphan rule]] dan o'tib o'tish.** Tashqi trait'ni tashqi tip uchun implement qilish mumkin emas, lekin newtype yaratib mahalliy qilish bilan mumkin. Backend traits source bu patternni orphan rule'dan keyingi tabiiy workaround sifatida preview qiladi.
2. **Type safety.** `Millimeters(u32)` va `Meters(u32)` — ikkalasi `u32` lekin aralashish mumkin emas.
3. **API yashirin tafsilotlarni hide qilish.** `UserId(u64)` — caller `u64`ning `+` operatorini ishlata olmaydi.
4. **Custom traits qo'shish.** Qo'shimcha behavior orginal tipga.

## Mental Model

```rust
struct Wrapper(InnerType);
//        ↑
//   yangi mahalliy tip — siz orphan rule bo'yicha trait implementatsiyalashingiz mumkin
```

Compile time'da `Wrapper` qatlami yo'qoladi — runtime overhead yo'q.

## Syntax and Examples

### Orphan rule'dan o'tish — `Display for Vec<T>`

`Vec<T>` ham, `Display` ham — ikkalasi `std`. To'g'ridan-to'g'ri:

```rust
impl Display for Vec<String> {     // E0117: orphan rule
    /* ... */
}
```

Newtype bilan:

```rust
use std::fmt;

struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

fn main() {
    let w = Wrapper(vec![String::from("hello"), String::from("world")]);
    println!("w = {w}");   // w = [hello, world]
}
```

`self.0` — tuple field index access (`Wrapper`'ning birinchi va yagona field'i).

### Type safety — units

```rust
struct Millimeters(u32);
struct Meters(u32);

fn travel(distance: Meters) { /* ... */ }

let mm = Millimeters(500);
// travel(mm);              // E0308 — Millimeters expected Meters
travel(Meters(2));           // OK
```

`u32` ikkalasi ham, lekin aralashtirish kompilyator tomonidan to'xtatiladi.

### API yashirish — `UserId`

```rust
pub struct UserId(u64);    // u64 private — caller ichidagi qiymatni ko'rmaydi

impl UserId {
    pub fn new(id: u64) -> Self { Self(id) }
    pub fn as_raw(&self) -> u64 { self.0 }
}
```

Caller `UserId::new(42)` qila oladi, lekin `id + 1` qila olmaydi (chunki `u64` interfeysi yashirilgan).

## Cheklovlar

- **Inner type'ning metodlari avtomatik kelmaydi.** `Wrapper(Vec<T>)` `.push()` chaqirsa — kompilyator topa olmaydi. Yechim:
  - Har metodni qo'lda delegate qilish: `pub fn push(&mut self, v: String) { self.0.push(v); }`.
  - Yoki [[deref-trait|Deref]] implement qilish — barcha metodlarni avtomatik o'tkazadi.

### Deref bilan transparent newtype

```rust
use std::ops::Deref;

struct Wrapper(Vec<String>);

impl Deref for Wrapper {
    type Target = Vec<String>;
    fn deref(&self) -> &Vec<String> { &self.0 }
}

fn main() {
    let w = Wrapper(vec![String::from("a")]);
    println!("{}", w.len());   // Vec::len chaqiriladi (Deref orqali)
}
```

Ammo Deref ishlatganingizda — newtype'ning type safety afzalliklari yo'qoladi.

## Type Alias bilan Taqqoslash

| Xususiyat | Newtype | [[type-alias|Type Alias]] |
|-----------|---------|---------------------------|
| Yangi tip | Ha | Yo'q (sinonim) |
| Type safety | Ha | Yo'q |
| Inner method'lar | Qo'lda yoki Deref | Avtomatik |
| Trait impl | Yangi tip uchun yangi | Inner uchun mavjudlar |
| Maqsad | Safety, abstraction | Qisqartirish |

Misol: `type Kilometers = i32;` (alias) — `i32` bilan aralashadi. `struct Kilometers(i32);` (newtype) — alohida tip, aralashtirib bo'lmaydi.

## Common Mistakes

- **`self.0` o'rniga `self.value` deb yozish.** Tuple struct field nomli emas — index orqali kirish.
- **Deref'ni avtomatik ekan deb hisoblash.** `Vec`'ning method'lari avtomatik kelmaydi; aniq implement kerak.
- **Newtype yaratish o'rniga `type alias` ishlatish.** `type Mm = u32` — `u32` bilan **aralashtirish mumkin** (alias semantik tip yaratmaydi). Type safety kerak bo'lsa newtype kerak.
- **Bir nechta field qo'shishga urinish.** Newtype — *bir* fieldli; bir nechta bo'lsa — bu boshqa pattern.

## Related Concepts

- [[orphan-rule|orphan rule]] — newtype bu qoidadan chetlab o'tish vositasi
- [[tuple-structs|tuple structs]] — newtype'ning sintaktik asosi
- [[deref-trait|Deref trait]] — transparent newtype yaratish
- [[type-checking|type checking]]
- [[domain-modeling|domain modeling]] — units, IDs, kabi
- [[custom-validation-types|custom validation types]] — newtype + invariant
- [[traits]]
- [[type-alias|type alias]] — sinonim, newtype bilan taqqoslash
- [[encapsulation]] — newtype encapsulation vositasi

## Sources

- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-traits]]
