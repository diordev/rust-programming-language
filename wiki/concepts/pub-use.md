---
title: "pub use"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, modules, api-design]
source_count: 2
---

# pub use

## Short Definition

`pub use` item'ni current scope'ga olib kiradi va uni tashqi code uchun shu scope'dan public qilib qayta export qiladi.

## Why It Matters

Re-export crate ichki module structure'ini public API structure'idan ajratishga yordam beradi. Internal code implementationga qulay bo'lgan folders/modules bilan turadi, crate users esa qulayroq va barqarorroq public path ishlatadi.

## Mental Model

`use` private shortcut yaratadi; `pub use` esa public shortcut yaratadi. Item original joyida qoladi, lekin tashqi code uni yangi public path orqali ham topa oladi.

## Syntax and Examples

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

External code endi `restaurant::hosting::add_to_waitlist()` pathini ishlatishi mumkin. Internal module hali `crate::front_of_house::hosting` ostida turadi.

## Common Mistakes

- `pub use` item'ni ko'chirib yuboradi deb o'ylash; u re-export qiladi.
- Re-export qilingan path ham public API contract ekanini unutish.
- `pub use` file compilationni belgilaydi deb o'ylash; file'larni crate'ga `mod` declarations ulaydi.

## Related Concepts

- [[use-declarations|use declarations]]
- [[public-api|public API]]
- [[api-design|API design]]
- [[paths]]
- [[module]]
- [[module-tree|module tree]]
- [[privacy]]
- [[pub-keyword|pub keyword]]

## Sources

- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[7-5-separating-modules-into-different-files-the-rust-programming-language]]
