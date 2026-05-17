---
title: "18.3. Implementing an Object-Oriented Design Pattern"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, design-patterns, state-pattern]
source_count: 1
---

# 18.3. Implementing an Object-Oriented Design Pattern

## Learning Goal

State design pattern'ni OOP usulida (`Box<dyn State>`) va Rust-idiomatic usulida (holatlarni turlar sifatida) implement qilish. Ikkala yondashuvning tradeoff'larini anglash.

## Main Ideas

### Blog Post workflow

```
Draft ──request_review──→ PendingReview ──approve──→ Published
  ↑ (approve → ta'sirsiz)       ↑ (approve'siz)
```

- Draft / PendingReview: `content()` → `""`
- Published: `content()` → haqiqiy matn

### Yondashuv 1 — OOP State Pattern

`Post` holatni `Option<Box<dyn State>>` sifatida saqlaydi. Har bir holat o'z tranzitsiyasini ta'minlaydi:

```rust
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

trait State {
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    fn approve(self: Box<Self>) -> Box<dyn State>;
    fn content<'a>(&self, post: &'a Post) -> &'a str { "" }
}
```

**`self: Box<Self>` receiver** — eski holatni consume qiladi, yangi holat qaytaradi:

```rust
struct Draft {}
impl State for Draft {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        Box::new(PendingReview {})  // Draft → PendingReview
    }
    fn approve(self: Box<Self>) -> Box<dyn State> { self }  // ta'sirsiz
}
```

**`Option::take()` triki** — struct field'dan ownership olish:

```rust
pub fn request_review(&mut self) {
    if let Some(s) = self.state.take() {  // state → None, s → ownership
        self.state = Some(s.request_review())
    }
}
// `self.state = self.state.request_review()` yozib bo'lmaydi:
// &mut self orqali self.state borrow'da, ayni vaqtda uni move qilib bo'lmaydi
```

**Content delegatsiyasi** — state o'zi javob beradi:

```rust
pub fn content(&self) -> &str {
    self.state.as_ref().unwrap().content(self)
    // as_ref(): Option<Box<dyn State>> → Option<&Box<dyn State>>
    // unwrap(): kafolatlangan Some (invariant: state doim Some)
    // content(self): state → post'ga reference uzatadi, lifetime bog'liq
}
```

`Published` holatida `content` override:

```rust
impl State for Published {
    fn content<'a>(&self, post: &'a Post) -> &'a str {
        &post.content   // haqiqiy matn
    }
    // request_review, approve → self (o'zgarmaydi)
}
```

### Yondashuv 2 — Type-encoded States (Rust idiomatic)

Har bir holat → alohida tur. `content()` metodi **faqat** `Post`da (published holat):

```rust
pub struct Post        { content: String }  // published
pub struct DraftPost   { content: String }  // content() yo'q!
pub struct PendingReviewPost { content: String }  // content() yo'q!

impl Post {
    pub fn new() -> DraftPost { DraftPost { content: String::new() } }
    pub fn content(&self) -> &str { &self.content }
}
impl DraftPost {
    pub fn add_text(&mut self, text: &str) { self.content.push_str(text); }
    pub fn request_review(self) -> PendingReviewPost {
        PendingReviewPost { content: self.content }  // self consumed
    }
}
impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post { content: self.content }  // self consumed
    }
}
```

```rust
let mut post = Post::new();       // → DraftPost
post.add_text("...");
let post = post.request_review(); // DraftPost → PendingReviewPost (shadowing)
let post = post.approve();        // PendingReviewPost → Post
println!("{}", post.content());   // ✓

// post.content() DraftPost uchun? → compile error: method not found
```

### Tradeoff solishtirish

| | OOP State Pattern | Type-encoded States |
|---|---|---|
| Xatolar | Runtime | **Compile-time** |
| `main` uslubi | `post.x(); post.y();` | `let post = post.x(); let post = post.y();` |
| Kengayish | 1 yangi struct + trait impl | Ko'proq turlar va metodlar |
| Encapsulation | Yuqori — tranzitsiya ichki | Past — caller boshqaradi |
| Rust idiomatic | Kam | **Ko'p** |

## Concepts

- [[state-pattern|State design pattern]] — `Box<dyn State>`, holat delegatsiyasi
- [[typestate-pattern|Typestate pattern]] — holat turlar sifatida, compile-time kafolat
- [[trait-object|trait object]] — `Box<dyn State>`
- [[dynamic-dispatch|dynamic dispatch]] — State metod chaqiruvlari

## Examples

- [[blog-post-state-pattern]] — OOP usuli to'liq implementatsiya
- [[blog-post-typestate]] — Type-encoded holat to'liq implementatsiya

## Exercises

1. OOP versiyaga `reject` qo'shing: `PendingReview → Draft`.
2. Ikki marta `approve` talab qilsin: `PendingReview` ichida `approval_count: u32` field.
3. Typestate versiyada ham `reject` implement qiling.

## Review Questions

1. `self: Box<Self>` receiver nima uchun kerak? `self` dan farqi?
2. `Option::take()` nima uchun ishlatiladi? Alternativi bormi?
3. `state.as_ref().unwrap().content(self)` da `as_ref()` nima uchun kerak?
4. Typestate pattern'da `DraftPost::content()` yo'qligi qanday foyda beradi?
5. Qachon OOP state pattern, qachon typestate afzalroq?

## Related Pages

- [[18-object-oriented-programming|18. OOP]] — umumiy ko'rinish
- [[18-2-using-trait-objects|18.2]] — trait objects asoslari
- [[wiki/sources/18-3-implementing-an-object-oriented-design-pattern|Source: 18.3]]
