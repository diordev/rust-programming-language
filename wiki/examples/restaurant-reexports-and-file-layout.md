---
title: "Restaurant re-exports and file layout"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, modules, api-design]
source_count: 2
---

# Restaurant re-exports and file layout

## Goal

[[pub-use|pub use]], [[mod-declarations|mod declarations]], va [[module-file-layout|module file layout]] birga ishlaganda internal organization va external [[public-api|public API]] qanday ajralishini ko'rsatish.

## Code

File layout:

```text
restaurant/
  src/
    lib.rs
    front_of_house.rs
    front_of_house/
      hosting.rs
```

`src/lib.rs`:

```rust
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

`src/front_of_house.rs`:

```rust
pub mod hosting;
```

`src/front_of_house/hosting.rs`:

```rust
pub fn add_to_waitlist() {}
```

## Explanation

`mod front_of_house;` compilerga root child module borligini bildiradi va body'ni `src/front_of_house.rs`dan topadi. `pub mod hosting;` esa `hosting` child module'ini `src/front_of_house/hosting.rs`dan topishga olib keladi.

`pub use crate::front_of_house::hosting;` internal module'ni root public API'da re-export qiladi. Shuning uchun crate users `restaurant::hosting::add_to_waitlist()` pathini ishlatishi mumkin, internal file layout esa `front_of_house` ostida qoladi.

`use` yoki `pub use` qaysi file compile bo'lishini belgilamaydi. File'larni module tree'ga `mod` declarations ulaydi; `use` pathni scope ichida qisqartiradi.

## Related Pages

- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language]]
- [[7-5-separating-modules-into-different-files-the-rust-programming-language]]
- [[pub-use|pub use]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]
- [[use-declarations|use declarations]]
- [[module-tree|module tree]]
- [[public-api|public API]]
