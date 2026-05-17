---
title: "Test Macros"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, assert, macros]
source_count: 1
---

# Test Macros

## Short Definition

`assert!`, `assert_eq!`, `assert_ne!` — Rust test funksiyalarida natijalarni tekshirish uchun ishlatiladigan standart library macro'lari. Shart bajarilmasa panic qilib testni FAILED qiladi.

## Why It Matters

Test failure'ni aniq va foydali xabar bilan ko'rsatish debugging'ni osonlashtiradi. `assert_eq!` ikkala qiymatni ham chiqaradi — bu `assert!(a == b)`dan ancha informativroq.

## Mental Model

Har uch macro ostida `panic!` turadi. Shart bajarilmasa panic → test thread o'ladi → runner FAILED deb belgilaydi. Shuning uchun test ortida hech qanday maxsus mexanizm yo'q — oddiy Rust panicking.

## Syntax and Examples

### `assert!`

Boolean ifodani tekshiradi. `false` bo'lsa panic.

```rust
assert!(larger.can_hold(&smaller));
assert!(!smaller.can_hold(&larger));

// Custom xabar bilan
assert!(
    result.contains("Carol"),
    "Greeting did not contain name, value was `{result}`"
);
```

### `assert_eq!`

Ikkita qiymat tengligini tekshiradi. Xato bo'lganda ikkala qiymatni chiqaradi.

```rust
assert_eq!(result, 4);
assert_eq!(add_two(2), 4);
```

Xato output:
```
assertion `left == right` failed
  left: 5
 right: 4
```

Argument tartibi: Rusтda `left` / `right` deyiladi (`expected` / `actual` emas). Tartib natijaga ta'sir qilmaydi.

### `assert_ne!`

Ikkita qiymat tengmasligini tekshiradi.

```rust
assert_ne!(result, 0);  // result 0 bo'lmasligi kerak
```

Qiymat nima bo'lishini bilmasak, lekin nima bo'lmasligini bilsak foydali.

### Custom failure messages

Uch macro ham qo'shimcha argumentlar qabul qiladi — `format!` kabi formatlangan xabar:

```rust
assert_eq!(
    greeting("Carol"),
    "Hello Carol!",
    "Expected greeting to include name"
);
```

### `PartialEq + Debug` talabi

`assert_eq!` va `assert_ne!` xato bo'lganda debug formatting ishlatadi. Shuning uchun solishtiriladigan typlar `PartialEq` va `Debug` implement qilishi kerak.

Custom struct/enum uchun:

```rust
#[derive(PartialEq, Debug)]
struct Point { x: i32, y: i32 }

assert_eq!(Point { x: 1, y: 2 }, Point { x: 1, y: 2 });
```

## Common Mistakes

- `assert_eq!(kutilgan, natija)` va `assert_eq!(natija, kutilgan)` tartibini aralashtirib o'ylash — Rust uchun farq yo'q, lekin `left`/`right` xabarda qaysi qiymat ekanini bilish kerak.
- Custom struct uchun `#[derive(PartialEq, Debug)]` yozmaslik — compile error beradi.
- `assert!`da murakkab shart yozib, qiymatlarni ko'rmasdan qolish — o'rniga `assert_eq!` ishlating.

## Related Concepts

- [[testing]]
- [[should-panic]]
- [[debug-trait|Debug trait]]
- [[partial-ord|PartialEq]] — (aslida `PartialEq`, ammo `PartialOrd` bilan birga turadi)
- [[panic]]
- [[format-macro|format! macro]]

## Sources

- [[11-1-how-to-write-tests]]
