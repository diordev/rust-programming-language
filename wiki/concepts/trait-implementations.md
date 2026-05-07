---
title: "Trait Implementations"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, traits, impl]
source_count: 1
---

# Trait Implementations

## Short Definition

Trait implementation muayyan type uchun trait contractini bajaradigan `impl Trait for Type` block.

## Why It Matters

Trait implementation shared behaviorni concrete type'ga bog'laydi. Shu implementationdan keyin type trait methodsni regular method kabi chaqira oladi va trait bounds talab qilgan joylarda ishlatiladi.

## Mental Model

`impl Summary for SocialPost` "SocialPost Summary behaviorini beradi" degani. Block ichida trait definition talab qilgan methods body bilan yoziladi. Agar trait default method bersa, implementation empty bo'lishi ham mumkin.

External crate trait methodini call qilishi uchun type bilan birga traitni ham scope'ga olib kirishi kerak.

## Syntax and Examples

```rust
impl Summary for SocialPost {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

Default methodni ishlatish:

```rust
impl Summary for NewsArticle {}
```

Using trait method from another crate:

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

## Common Mistakes

- `impl Type` regular method blocki bilan `impl Trait for Type`ni chalkashtirish.
- Trait method signature'ini definition bilan aynan mos yozmaslik.
- Trait method call uchun trait scope'da bo'lishi kerak bo'lgan holatlarni unutish.
- [[orphan-rule|Orphan rule]] sabab external traitni external type uchun implement qilib bo'lmasligini unutish.

## Related Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[impl-block|impl block]]
- [[orphan-rule|orphan rule]]
- [[default-trait-implementations|default trait implementations]]
- [[public-api|public API]]

## Sources

- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
