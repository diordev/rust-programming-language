---
title: "Clippy approx constant example"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, clippy, lints]
source_count: 1
---

# Clippy approx constant example

Clippy approximate mathematical constant ishlatilganini ushlaydi.

```rust
fn main() {
    let x = 3.1415;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

Improved variant:

```rust
fn main() {
    let x = std::f64::consts::PI;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

## Why It Matters

Clippy correctnessga yaqin quality issue'larni ham ko'rsatadi, faqat syntax yoki type xatolarni emas.

## Related

- [[clippy]]
- [[readable-code|readable code]]
