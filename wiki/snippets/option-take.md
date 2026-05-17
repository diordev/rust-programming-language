---
title: "Option::take()"
type: snippet
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, option, ownership, snippet]
source_count: 1
---

# Option::take()

## Maqsadi

`Option<T>` ichidan qiymatni **ownership bilan** olib chiqib, joyida `None` qoldirish. `&mut self` orqali ishlaydi — lekin `T` qiymati move bo'ladi.

## Signatura

```rust
impl<T> Option<T> {
    pub fn take(&mut self) -> Option<T>;
}
```

`self` mutable reference, qaytaradigan qiymat — original `Option<T>`. Ko'rsatilgan joy `None` ga o'rnatiladi.

## Asosiy Foydalanish: Struct Field'dan Ownership Olish

Eng keng tarqalgan kontekst — [[state-pattern|state pattern]] da: `&mut self` bo'yicha metod field qiymatini iste'mol qilishi kerak (transition uchun), lekin Rust `&mut self` orqali field'ni move qila olmaydi. `Option::take()` bu cheklov atrofidan o'tib o'tadi:

```rust
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

impl Post {
    pub fn request_review(&mut self) {
        if let Some(s) = self.state.take() {
            //                  ↑ field'dan Box<dyn State> chiqib oladi,
            //                    self.state = None bo'lib qoladi
            self.state = Some(s.request_review());
        }
    }
}
```

`self.state.take()` qilmasdan to'g'ridan-to'g'ri `s.request_review()` chaqirib bo'lmaydi — `s` `&mut self` orqali borrowed bo'lib qoladi.

## Boshqa Foydalanishlar

### Sentinel almashtirish

```rust
let mut name: Option<String> = Some(String::from("Ada"));
let owned = name.take();      // owned = Some("Ada"), name = None
assert!(name.is_none());
```

### Cleanup logic

```rust
struct Resource { handle: Option<Handle> }

impl Drop for Resource {
    fn drop(&mut self) {
        if let Some(h) = self.handle.take() {
            h.close();   // owned Handle bilan close()
        }
    }
}
```

## Mem::replace bilan taqqoslash

`Option::take()` aslida `mem::replace(self, None)` ning ergonomik shorthand'i. Boshqa default qiymat kerak bo'lsa:

```rust
use std::mem;
let old = mem::replace(&mut field, new_value);
```

## Common Mistakes

- `take()` qaytargan qiymatni e'tiborga olmaslik: `option.take();` qiymatni o'qimasdan tashlab yuboradi va `None` qoldiradi.
- `clone()` o'rniga `take()` ishlatish — agar original kerak bo'lmasa, `take()` arzon (allokatsiya yo'q).
- `take()` dan keyin original `Option`'ni yana `Some` bilan to'ldirishni unutish — state machine bo'lsa muammo: holatsiz qoladi.

## Related Concepts

- [[option|Option]] — bazaviy enum
- [[state-pattern|State design pattern]] — `take()` ning klassik foydalanish konteksti
- [[ownership]] — qoidalardan o'tib ketish triki
- [[move-semantics]] — `take()` move qiladi
- [[drop]] — Drop impl ichida ham foydali

## Sources

- [[wiki/sources/18-3-implementing-an-object-oriented-design-pattern|18.3 OOP Design Pattern]]
