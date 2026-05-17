---
title: "Blog Post — Typestate Pattern"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, design-patterns, typestate, example]
source_count: 1
---

# Blog Post — Typestate Pattern

## Description

Holatlarni alohida turlar sifatida ifodalash: `DraftPost`, `PendingReviewPost`, `Post`. Draft'dan content o'qish compile error — runtime tekshirish kerak emas.

## Source

[[wiki/sources/18-3-implementing-an-object-oriented-design-pattern]] — Listing 18-19 dan 18-21 gacha.

## To'liq implementatsiya

```rust
// lib.rs
pub struct Post { content: String }

pub struct DraftPost { content: String }

pub struct PendingReviewPost { content: String }

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
        PendingReviewPost { content: self.content }
    }
}

impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post { content: self.content }
    }
}
```

## Ishlatish (main.rs)

```rust
use blog::Post;

fn main() {
    let mut post = Post::new();                    // post: DraftPost

    post.add_text("I ate a salad for lunch today");

    let post = post.request_review();              // post: PendingReviewPost

    let post = post.approve();                     // post: Post

    assert_eq!("I ate a salad for lunch today", post.content());
}
```

## Compile-time xato namunasi

```rust
let draft = Post::new();          // DraftPost
println!("{}", draft.content());  // COMPILE ERROR:
// error[E0599]: no method named `content` found for struct `DraftPost`
```

Draft'dan content o'qishga urinish dastur yozilish vaqtida ushlanadi — runtime'da tekshirib o'tirmaslik kerak.

## OOP State Pattern bilan farq

```rust
// OOP State Pattern — bir xil `post` variable:
let mut post = Post::new();
post.add_text("...");
post.request_review();    // post: hali Post
post.approve();           // post: hali Post
post.content()            // runtime: state tekshiriladi

// Typestate — har o'tishda yangi tur:
let mut post = Post::new();          // DraftPost
post.add_text("...");
let post = post.request_review();    // PendingReviewPost — shadowing
let post = post.approve();           // Post — shadowing
post.content()                       // compile-time kafolat
```

## Concepts Used

- [[typestate-pattern|Typestate pattern]] — holat turlar sifatida
- [[ownership]] — tranzitsiya metodlari `self` consume qiladi
- [[shadowing]] — `let post = post.x()` pattern
- [[move-semantics|move semantics]] — eski tur consumed

## Related Pages

- [[blog-post-state-pattern]] — OOP alternativasi
- [[wiki/chapters/18-3-implementing-an-object-oriented-design-pattern]]
