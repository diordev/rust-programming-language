---
title: "Blog Post — OOP State Pattern"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, design-patterns, state-pattern, example]
source_count: 1
---

# Blog Post — OOP State Pattern

## Description

`Box<dyn State>` bilan OOP state pattern: Draft → PendingReview → Published. Holat tranzitsiyalari `Post` ichida kapsullangan.

## Source

[[wiki/sources/18-3-implementing-an-object-oriented-design-pattern]] — Listing 18-11 dan 18-18 gacha.

## To'liq implementatsiya

```rust
// lib.rs
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

impl Post {
    pub fn new() -> Post {
        Post { state: Some(Box::new(Draft {})), content: String::new() }
    }
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }
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
    fn content<'a>(&self, _post: &'a Post) -> &'a str { "" }
}

struct Draft {}
impl State for Draft {
    fn request_review(self: Box<Self>) -> Box<dyn State> { Box::new(PendingReview {}) }
    fn approve(self: Box<Self>) -> Box<dyn State> { self }
}

struct PendingReview {}
impl State for PendingReview {
    fn request_review(self: Box<Self>) -> Box<dyn State> { self }
    fn approve(self: Box<Self>) -> Box<dyn State> { Box::new(Published {}) }
}

struct Published {}
impl State for Published {
    fn request_review(self: Box<Self>) -> Box<dyn State> { self }
    fn approve(self: Box<Self>) -> Box<dyn State> { self }
    fn content<'a>(&self, post: &'a Post) -> &'a str { &post.content }
}
```

## Ishlatish (main.rs)

```rust
use blog::Post;

fn main() {
    let mut post = Post::new();

    post.add_text("I ate a salad for lunch today");
    assert_eq!("", post.content());          // Draft — bo'sh

    post.request_review();
    assert_eq!("", post.content());          // PendingReview — bo'sh

    post.approve();
    assert_eq!("I ate a salad for lunch today", post.content());  // Published!
}
```

## Muhim texniklar

**`Option::take()` triki:**
```rust
// self.state = self.state.request_review() — XATO: borrow + move konflikti
if let Some(s) = self.state.take() {  // → None qoldiradi, s → ownership
    self.state = Some(s.request_review());
}
```

**`as_ref().unwrap()` delegatsiya:**
```rust
self.state.as_ref()  // Option<&Box<dyn State>> — ownership olmaydi
    .unwrap()        // &Box<dyn State> — doim Some (invariant)
    .content(self)   // state → post'ni argument sifatida oladi
```

## Concepts Used

- [[state-pattern|State design pattern]] — OOP implementation
- [[trait-object|trait object]] — `Box<dyn State>`
- [[dynamic-dispatch|dynamic dispatch]] — state method calls
- [[option]] — `Option::take()` triki

## Related Pages

- [[blog-post-typestate]] — Rust-idiomatic alternativi
- [[wiki/chapters/18-3-implementing-an-object-oriented-design-pattern]]
