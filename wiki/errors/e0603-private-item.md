---
title: "E0603 private item"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, modules, privacy]
source_count: 1
---

# E0603 private item

## Symptom

Compiler `error[E0603]` beradi: module, function, struct field yoki boshqa item private bo'lgani uchun path orqali ishlatib bo'lmaydi.

Chapter 7.3 misolida:

```text
error[E0603]: module `hosting` is private
```

Keyingi bosqichda module public qilingach, function private qolgani uchun:

```text
error[E0603]: function `add_to_waitlist` is private
```

## Cause

Rust item'lari private by default. [[paths|Path]] syntactically to'g'ri bo'lsa ham, [[privacy]] qoidalari ruxsat bermasa item'ga kirib bo'lmaydi.

`pub mod hosting` faqat module nomini public qiladi; module ichidagi `fn add_to_waitlist` hali private bo'lib qoladi.

## Fix Pattern

Public API sifatida ochilishi kerak bo'lgan har bir level'ni `pub` bilan belgilang:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
}
```

Lekin har bir narsani `pub` qilish shart emas. Avval item haqiqatan public API bo'lishi kerakmi, shuni aniqlang.

## Minimal Example

Failing:

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
}
```

Fixed:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
}
```

## Related Concepts

- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[public-api|public API]]
