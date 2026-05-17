---
title: "18.3. Implementing an Object-Oriented Design Pattern"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, design-patterns, state-pattern]
source_count: 1
---

# 18.3. Implementing an Object-Oriented Design Pattern

## Source

- URL: https://doc.rust-lang.org/stable/book/ch18-03-oo-design-patterns.html
- Raw: `raw/books/the_rust_programming_language/18.3. Implementing an Object-Oriented Design Pattern.md`

## Detailed Summary

Bu bo'lim **State design pattern**ni ikki xil usulda implement qilishni ko'rsatadi: avval klassik OOP usulida, keyin Rust'ning type system kuchidan foydalangan holda.

### Vazifa: blog post workflow

```
Draft → PendingReview → Published
```

Qoidalar:
- Draft va PendingReview da `content()` bo'sh string qaytaradi
- Faqat Published post haqiqiy matni ko'rsatadi
- Noto'g'ri o'tishlar (Draft'ni approve qilmoq) ta'sir qilmaydi

### Yondashuv 1: OOP State Pattern

`Post` ichida `Option<Box<dyn State>>` field — joriy holatni saqlaydi:

```rust
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}
trait State {
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    fn approve(self: Box<Self>) -> Box<dyn State>;
    fn content<'a>(&self, post: &'a Post) -> &'a str { "" }  // default
}
```

`self: Box<Self>` receiver — faqat `Box` ichida bo'lganda chaqiriladi, ownership oladi → eski holat consumed bo'ladi.

`Option::take()` triki — field'dan ownership olish uchun:

```rust
pub fn request_review(&mut self) {
    if let Some(s) = self.state.take() {   // state → None
        self.state = Some(s.request_review()) // yangi holat
    }
}
```

`self.state = self.state.request_review()` yozib bo'lmaydi — `self.state`dan borrow olib, ayni vaqtda uni o'zgartirish mumkin emas. `take()` triki yechim.

`content()` delegatsiyasi:

```rust
pub fn content(&self) -> &str {
    self.state.as_ref().unwrap().content(self)
}
// as_ref(): Option<Box<dyn State>> → Option<&Box<dyn State>> — ownership olmaydi
```

State'lar: `Draft` (approve va request_review'da `self` qaytaradi), `PendingReview` (approve → Published), `Published` (request_review va approve'da `self`, content → &post.content).

**Qulaylik**: yangi holat qo'shish uchun faqat yangi struct + trait impl. `Post` kodi o'zgarmaydi.

**Kamchiliklar**:
- Holatlar bir-biriga bog'liq (coupling): yangi `Scheduled` holat qo'shilsa `PendingReview` ham o'zgarishi kerak
- Logika takrorlanishi: `request_review` va `approve` metodlari `Post`da deyarli bir xil
- Default implementation muammosi: `self: Box<Self>` receiver bilan `State` trait object sifatida `self`ni qaytarish dyn compatibility buzmaydi, lekin cheklovlar bor

### Yondashuv 2: Type-encoded States (Rust idiomatic)

Holatlarni turlar sifatida ifodalash — invalide holatlar compile time'da yo'q qilinadi:

```rust
pub struct Post        { content: String }  // faqat published
pub struct DraftPost   { content: String }  // content() metodi YO'Q
pub struct PendingReviewPost { content: String }  // content() metodi YO'Q

impl Post {
    pub fn new() -> DraftPost { DraftPost { content: String::new() } }
    pub fn content(&self) -> &str { &self.content }
}
impl DraftPost {
    pub fn add_text(&mut self, text: &str) { self.content.push_str(text); }
    pub fn request_review(self) -> PendingReviewPost {
        PendingReviewPost { content: self.content }
    }
}
impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post { content: self.content }
    }
}
```

Main:
```rust
let mut post = Post::new();           // DraftPost
post.add_text("salad for lunch");
let post = post.request_review();     // PendingReviewPost (shadowing)
let post = post.approve();            // Post
assert_eq!("salad for lunch", post.content());
```

**Afzallik**: `DraftPost::content()` yo'q → kompilyator draft'dan content o'qishni rad etadi. Publish qilinmagan post'dan content o'qish **compile error**. Runtime'da tekshirish kerak emas.

**Kamchilik**: `main`da `let post = post.request_review()` kabi shadowing kerak — OOP state pattern'da `post.request_review(); post.content()` deb davom etish mumkin edi.

### Taqqoslash

| | OOP State Pattern | Type-encoded States |
|---|---|---|
| Xatolar | Runtime (noto'g'ri state tekshiriladi) | Compile-time |
| API ergonomics | `post.request_review()` — bir xil `post` o'zgaruvchi | `let post = post.request_review()` — yangi binding |
| Kengayish | Yangi struct + trait impl | Ko'proq struct va funksiyalar |
| Encapsulation | Yuqori — state o'zgarishi `Post` ichida | Past — caller binding bilan boshqaradi |
| Rust idiomatic | Kam | Ko'p |

## Key Concepts

- [[state-pattern|State design pattern]] — OOP state pattern Rust implementatsiyasi
- [[typestate-pattern|Typestate pattern]] — holat → tur, invalide holat compile error
- [[trait-object|trait object]] — `Box<dyn State>` holat sifatida
- [[option-take|Option::take()]] — field'dan ownership olish triki
- [[dynamic-dispatch|dynamic dispatch]] — State trait object

## Code Examples

- [[blog-post-state-pattern]] — OOP usuli: Post + Draft/PendingReview/Published
- [[blog-post-typestate]] — Rust usuli: DraftPost/PendingReviewPost/Post

## Exercises or Practice Ideas

1. State pattern'ga `reject` metodi qo'shing: `PendingReview → Draft`.
2. `Published` uchun ikki marta `approve` kerak qilib o'zgartiring.
3. Type-encoded versiyada `reject` metodi: `PendingReviewPost → DraftPost`.

## Questions Raised

- `self: Box<Self>` receiver qachon foydali? Oddiy `self` dan farqi nima?
- Typestate pattern real loyihalarda qachon ishlatiladi?

## Links To Update

- [[wiki/chapters/18-3-implementing-an-object-oriented-design-pattern]] — yangi chapter page
- [[state-pattern]] — yangi concept page
- [[typestate-pattern]] — yangi concept page
