---
title: "should_panic"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, panic, attributes]
source_count: 1
---

# should_panic

## Short Definition

`#[should_panic]` — test funksiyasi panic qilishi kerakligini bildiruvchi attribute. Kod panic qilsa test o'tadi; panic bo'lmasa FAILED.

## Why It Matters

Ba'zan kodning xato holatlarni to'g'ri boshqarishini, ya'ni kerakli joyda panic qilishini tekshirish kerak. Masalan, `Guess::new(200)` 1–100 diapazonidan tashqarida bo'lgani uchun panic qilishi kerak. `#[should_panic]` shu xatti-harakatni testlashning rasmiy usuli.

## Syntax and Examples

### Asosiy ishlatish

```rust
#[test]
#[should_panic]
fn greater_than_100() {
    Guess::new(200);
}
```

Test o'tadi — `Guess::new(200)` panic qiladi.  
Test FAILED bo'ladi — `Guess::new(200)` panic qilmasa.

### `expected` parametri bilan aniqlashtirish

`#[should_panic]` noaniq — istalgan sababdan panic qilsa o'tadi. `expected` substringni tekshiradi:

```rust
#[test]
#[should_panic(expected = "less than or equal to 100")]
fn greater_than_100() {
    Guess::new(200);
}
```

Test o'tadi — panic xabari `"less than or equal to 100"` substringini o'z ichiga olsa.  
Test FAILED bo'ladi — panic xabari boshqa substring bo'lsa:

```
note: panic did not contain expected string
      panic message: "Guess value must be greater than or equal to 1, got 200."
 expected substring: "less than or equal to 100"
```

### `Result<T, E>` bilan mos kelmaydi

`Result<T, E>` returning testlarda `#[should_panic]` ishlatib bo'lmaydi. O'rniga:

```rust
assert!(value.is_err());
```

## Common Mistakes

- `expected` yozmay noaniq `#[should_panic]` ishlatish — boshqa sababdan panic qilsa ham o'tadi, bug yashirinishi mumkin.
- `Result<T, E>` test bilan birga ishlatishga urinish.

## Related Concepts

- [[testing]]
- [[test-macros]]
- [[panic]]
- [[cfg-test]]
- [[guess-validation-type]]
- [[result|Result]]

## Sources

- [[11-1-how-to-write-tests]]
