---
title: "Typestate Pattern"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, design-patterns, type-system]
source_count: 1
---

# Typestate Pattern

## Short Definition

Typestate pattern — ob'ektning turli holatlari alohida Rust **turlar** sifatida ifodalanadi. Invalide holat o'tishlari compile-time'da imkonsiz: mavjud bo'lmagan metodlar chaqirib bo'lmaydi va noto'g'ri tip accept qilinmaydi.

## Why It Matters

OOP state pattern runtime'da noto'g'ri holatlarni tutadi — masalan, Draft post'ning content'ini ko'rsatmaslik uchun runtime tekshirish kerak. Typestate pattern esa bu xatolarni compile-time'da imkonsiz qiladi: `DraftPost` da `content()` metodi umuman yo'q. Rust'ning ownership va type system'i bu pattern'ni tabiiy ifodalashga imkon beradi.

## Mental Model

Builder pattern bilan o'xshashlik: `HttpRequestBuilder::body()` faqat `headers()` chaqirilgandan keyin ishlaydi — `WithHeaders` turi qaytariladi. `body()` faqat `WithHeaders`da mavjud. Noto'g'ri tartib → compile error.

## Syntax and Examples

### Blog post typestate

```rust
pub struct Post               { content: String }  // faqat published holat
pub struct DraftPost          { content: String }  // content() yo'q
pub struct PendingReviewPost  { content: String }  // content() yo'q

// Faqat Post::new() bilan boshlanadi → DraftPost
impl Post {
    pub fn new() -> DraftPost {
        DraftPost { content: String::new() }
    }
    pub fn content(&self) -> &str { &self.content }
}

impl DraftPost {
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }
    pub fn request_review(self) -> PendingReviewPost {
        PendingReviewPost { content: self.content }  // DraftPost consumed
    }
}

impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post { content: self.content }  // PendingReviewPost consumed
    }
}
```

### Ishlatish

```rust
let mut post = Post::new();           // post: DraftPost
post.add_text("salad for lunch");

let post = post.request_review();     // post: PendingReviewPost (shadowing)
                                      // eski DraftPost consumed — yo'q bo'ldi

let post = post.approve();            // post: Post
println!("{}", post.content());       // ✓ faqat Published da mavjud

// Draft'dan content o'qishga urinish:
let draft = Post::new();
println!("{}", draft.content());      // COMPILE ERROR: method `content` not found
```

### Tranzitsiyalar ownership bilan

`self` (owned) qabul qiluvchi metodlar eski tipni "iste'mol qiladi":

```rust
pub fn request_review(self) -> PendingReviewPost { ... }
// DraftPost consumed → endi draft variable yo'q
// PendingReviewPost yangi variable sifatida yaratiladi
```

`let post = post.request_review()` — shadowing orqali bir xil nom saqlanadi.

## OOP State Pattern bilan taqqoslash

| Mezon | OOP State Pattern | Typestate Pattern |
|---|---|---|
| Xato qachon | Runtime | **Compile-time** |
| `main` ergonomics | `post.x(); post.y();` | `let post = post.x();` |
| Encapsulation | Yuqori — ichki | Past — caller boshqaradi |
| Rust idiomatic | Kam | **Ko'p** |
| Holat soni | Ko'p bo'lsa ham bitta `Post` | Har holat uchun alohida tur |
| Reject metodi | Oddiy qo'shish mumkin | Yangi tur kerak bo'lishi mumkin |

## Common Mistakes

- **`let mut post = post.request_review()`** — `mut` shart emas, agar o'zgartirilmasa.
- **Holatlar ortib ketishi**: ko'p holat bo'lsa ko'p tur → kod murakkablashadi. 3-5 holatdan oshsa OOP pattern afzalroq.

## Related Concepts

- [[state-pattern|State design pattern]] — OOP alternativasi
- [[ownership]] — tranzitsiyalar `self` ownership bilan ishlaydi
- [[move-semantics|move semantics]] — eski holat consumed, yangi holat yaratiladi
- [[type-checking|type checking]] — compile-time invalide holat oldini oladi
- [[shadowing]] — `let post = post.x()` pattern

## Sources

- [[wiki/sources/18-3-implementing-an-object-oriented-design-pattern]]
