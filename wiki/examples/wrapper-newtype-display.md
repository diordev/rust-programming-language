---
title: "Wrapper newtype — Display for Vec<String>"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, newtype, orphan-rule]
source_count: 1
---

# Wrapper Newtype — Display for `Vec<String>`

## Muammo

`Display` trait'ini `Vec<String>` uchun to'g'ridan-to'g'ri implement qila olmaymiz:

```rust
impl std::fmt::Display for Vec<String> {
    /* ... */
}
```

```text
error[E0117]: only traits defined in the current crate can be implemented
              for arbitrary types
```

[[orphan-rule|Orphan rule]]: trait yoki tip — kamida bittasi mahalliy bo'lishi shart. `Display` `std`'da, `Vec<String>` `std`'da → ikkalasi tashqi.

## Listing 20-24 — Newtype Bilan Yechim

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
    println!("w = {w}");
    // w = [hello, world]
}
```

`Wrapper` — yangi mahalliy tip → kompilyator orphan rule'ga rioya qildi deb hisoblaydi.

## Tahlil

- `struct Wrapper(Vec<String>)` — tuple struct, bitta field.
- `self.0` — birinchi (yagona) field'ga kirish.
- `self.0.join(", ")` — `Vec<String>::join` ishlatiladi.
- Compile time'da `Wrapper` qatlami yo'qoladi → runtime overhead 0.

## Cheklov: Inner Method'lar Avtomatik Kelmaydi

```rust
let w = Wrapper(vec![String::from("a")]);
w.push(String::from("b"));   // E0599: no method `push` found
```

`Vec`'ning method'lari `Wrapper` uchun ko'rinmaydi.

### Yechim 1 — Qo'lda delegate

```rust
impl Wrapper {
    pub fn push(&mut self, s: String) { self.0.push(s); }
    pub fn len(&self) -> usize { self.0.len() }
    // ... har metod alohida
}
```

### Yechim 2 — `Deref` implement qilish

```rust
use std::ops::Deref;

impl Deref for Wrapper {
    type Target = Vec<String>;
    fn deref(&self) -> &Vec<String> { &self.0 }
}

let w = Wrapper(vec![String::from("a")]);
println!("{}", w.len());   // Vec::len, Deref orqali
```

Lekin Deref ishlatganingizda — newtype'ning type safety afzalliklari pasayadi (har joy `&Vec<String>` sifatida ko'rinadi).

## Foydalanish Sohalari

- **Tashqi trait + tashqi tip:** orphan rule chetlab o'tish (yuqoridagi misol).
- **Type-safe units:** `Millimeters(u32)` vs `Meters(u32)`.
- **API yashirish:** `UserId(u64)` — caller `+`, `*` ishlata olmaydi.
- **Domain modeling:** `EmailAddress(String)`, `PostalCode(String)` — `String` o'rniga aniq tiplar.

## Related

- [[newtype-pattern|newtype pattern]]
- [[orphan-rule|orphan rule]]
- [[tuple-structs|tuple structs]]
- [[deref-trait|Deref trait]]
- [[display-formatting|Display formatting]]
- [[traits]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
