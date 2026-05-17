---
title: "State Design Pattern"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, design-patterns]
source_count: 1
---

# State Design Pattern

## Short Definition

State pattern — ob'ektning xatti-harakati uning ichki holatiga qarab o'zgaradigan dizayn pattern. Rust'da `Option<Box<dyn State>>` bilan implement qilinadi: har bir holat o'z tranzitsiya qoidalarini biladi.

## Why It Matters

State pattern'ning asosiy foydasi: holat o'zgarish qoidalari tarqoq `match` ifodalari o'rniga har bir holat strukturasi ichida jamlangan. Yangi holat qo'shish uchun faqat yangi struct yoziladi — mavjud kod o'zgarmaydi. Bu katta biznes qoidalarini boshqarishda kodni tushunishni osonlashtiradi.

## Mental Model

Svetofor: qizil → yashil → sariq → qizil. Har bir rang keyingi rangni o'zi biladi. `Svetofor::o'zgartir()` faqat joriy holatga delegation qiladi — holat o'zi qaror qiladi.

## Syntax and Examples

### To'liq implementatsiya — blog post

```rust
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

impl Post {
    pub fn new() -> Post {
        Post { state: Some(Box::new(Draft {})), content: String::new() }
    }
    pub fn add_text(&mut self, text: &str) { self.content.push_str(text); }
    pub fn content(&self) -> &str {
        self.state.as_ref().unwrap().content(self)
    }
    pub fn request_review(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.request_review())
        }
    }
    pub fn approve(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.approve())
        }
    }
}

trait State {
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    fn approve(self: Box<Self>) -> Box<dyn State>;
    fn content<'a>(&self, _post: &'a Post) -> &'a str { "" }  // default: bo'sh
}

struct Draft {}
impl State for Draft {
    fn request_review(self: Box<Self>) -> Box<dyn State> { Box::new(PendingReview {}) }
    fn approve(self: Box<Self>) -> Box<dyn State> { self }  // ta'sirsiz
}

struct PendingReview {}
impl State for PendingReview {
    fn request_review(self: Box<Self>) -> Box<dyn State> { self }  // ta'sirsiz
    fn approve(self: Box<Self>) -> Box<dyn State> { Box::new(Published {}) }
}

struct Published {}
impl State for Published {
    fn request_review(self: Box<Self>) -> Box<dyn State> { self }
    fn approve(self: Box<Self>) -> Box<dyn State> { self }
    fn content<'a>(&self, post: &'a Post) -> &'a str { &post.content }
}
```

### `self: Box<Self>` receiver

```rust
fn request_review(self: Box<Self>) -> Box<dyn State>;
// - faqat Box ichida bo'lganda chaqiriladi
// - ownership oladi → eski holat consumed, qaytarib bo'lmaydi
// - yangi holat return qilinadi
```

Oddiy `self` bilan: `Box<Draft>` chaqirib bo'lmaydi. `&self` bilan: eski holatni consume qilib bo'lmaydi.

### `Option::take()` triki

```rust
// XATO — self.state borrow va move bir vaqtda:
self.state = Some(self.state.request_review()); // move qilib bo'lmaydi

// TO'G'RI — take() bilan:
if let Some(s) = self.state.take() {  // self.state → None, s → ownership
    self.state = Some(s.request_review());  // yangi holat o'rnatiladi
}
```

`Option::take()` — `&mut self` orqali field'dan qiymatni olib, o'rniga `None` qoldiradi.

### `as_ref().unwrap().content(self)`

```rust
self.state.as_ref().unwrap().content(self)
//  ^^^^^^ Option<Box<dyn State>> → Option<&Box<dyn State>>
//         ownership olmaydi — &self funksiyasida kerak
//                   ^^^^^^ kafolatlangan Some (invariant)
//                          ^^^^^^^ delegation: Post o'zini state'ga uzatadi
```

## Afzalliklari va kamchiliklari

**Afzallik:**
- Yangi holat → faqat yangi struct + impl, mavjud `Post` kodi o'zgarmaydi
- Holat qoidalari bir joyda jamlangan, `match` tarqalmaydi
- Noto'g'ri operatsiyalar (Draft'ni approve) ta'sir qilmaydi

**Kamchilik:**
- Holatlar orasida coupling: yangi holat qo'shilsa boshqalar o'zgarishi mumkin
- Logika takrorlanishi: `request_review` va `approve` `Post`da deyarli bir xil
- Rust type system'dan to'liq foydalanmaydi (invalide holat runtime'da emas)

**Alternativi**: [[typestate-pattern|Typestate pattern]] — invalide holat compile-time'da yo'q qilinadi.

## Common Mistakes

- **`self.state = self.state.request_review()`** — borrow va move konflikti; `Option::take()` kerak.
- **`state.content(self)` da ownership xatosi** — `as_ref()` ishlatmaslik; `state` `Option<Box<...>>`, ownership olmaydi.

## Related Concepts

- [[typestate-pattern|Typestate pattern]] — Rust-idiomatic alternativi
- [[trait-object|trait object]] — `Box<dyn State>` holat sifatida
- [[dynamic-dispatch|dynamic dispatch]] — state metod chaqiruvlari runtime'da
- [[encapsulation]] — holat tranzitsiyalari `Post` ichida yashiringan
- [[option]] — `Option::take()` triki uchun

## Sources

- [[wiki/sources/18-3-implementing-an-object-oriented-design-pattern]]
