---
title: "Summary Trait Example"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, traits]
source_count: 1
---

# Summary Trait Example

## Summary

Chapter 10.2 `Summary` traiti orqali shared behaviorni ko'rsatadi. `NewsArticle` va `SocialPost` turli data shape'lariga ega, lekin ikkalasi ham `summarize()` behaviorini beradi.

## Code

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct SocialPost {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub repost: bool,
}

impl Summary for SocialPost {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

Using the trait from a binary crate:

```rust
use aggregator::{SocialPost, Summary};

fn main() {
    let post = SocialPost {
        username: String::from("horse_ebooks"),
        content: String::from("of course, as you probably already know, people"),
        reply: false,
        repost: false,
    };

    println!("1 new post: {}", post.summarize());
}
```

## Explanation

- `Summary` trait method signatureni contract qiladi.
- `impl Summary for NewsArticle` va `impl Summary for SocialPost` concrete behavior beradi.
- Trait method regular method kabi chaqiriladi.
- External crate methodni call qilishi uchun trait ham scope'da bo'lishi kerak.

## Related Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[public-api|public API]]
- [[orphan-rule|orphan rule]]

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
