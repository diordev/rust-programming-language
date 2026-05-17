---
title: "Restaurant paths and privacy"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, modules, paths, privacy]
source_count: 1
---

# Restaurant paths and privacy

## Goal

[[absolute-path|Absolute path]], [[relative-path|relative path]], [[privacy]], va [[pub-keyword|pub]] birga ishlaganda path to'g'ri bo'lishi bilan access ruxsati alohida masala ekanini ko'rsatish.

## Code

Failing version:

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
    front_of_house::hosting::add_to_waitlist();
}
```

Fixed version:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    crate::front_of_house::hosting::add_to_waitlist();
    front_of_house::hosting::add_to_waitlist();
}
```

`super` example:

```rust
fn deliver_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::deliver_order();
    }

    fn cook_order() {}
}
```

Public struct va enum:

```rust
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }

    pub enum Appetizer {
        Soup,
        Salad,
    }
}
```

## Explanation

Failing versionda ikkala path ham `add_to_waitlist`ga to'g'ri boradi, lekin `hosting` private bo'lgani uchun [[e0603-private-item|E0603]] chiqadi. `pub mod hosting` qo'shilsa ham function private qoladi. Path ishlashi uchun pathdagi target va kerakli intermediate items privacy qoidalari bo'yicha accessible bo'lishi kerak.

`crate::front_of_house::hosting::add_to_waitlist()` crate rootdan boshlanadigan absolute path. `front_of_house::hosting::add_to_waitlist()` esa `eat_at_restaurant` turgan module'dan boshlanadigan relative path.

`super::deliver_order()` child module'dan parent module'ga chiqadi. Bu `back_of_house` va `deliver_order` birga ko'chishi mumkin bo'lgan refactorlarda foydali bo'lishi mumkin.

`pub struct Breakfast` struct type'ni public qiladi, lekin `seasonal_fruit` private qoladi. `pub enum Appetizer` esa `Soup` va `Salad` variantsini ham public qiladi.

## Related Pages

- [[7-3-paths-for-referring-to-an-item-in-the-module-tree]]
- [[paths]]
- [[absolute-path|absolute path]]
- [[relative-path|relative path]]
- [[super-keyword|super keyword]]
- [[privacy]]
- [[pub-keyword|pub keyword]]
- [[public-api|public API]]
- [[e0603-private-item|E0603 private item]]
