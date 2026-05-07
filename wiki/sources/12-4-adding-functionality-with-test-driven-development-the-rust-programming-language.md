---
title: "12.4. Adding Functionality with Test-Driven Development"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, tdd, testing, lifetimes, search]
source_count: 1
---

# 12.4. Adding Functionality with Test-Driven Development

## Source

- Manba: The Rust Programming Language, 12.4
- URL: https://doc.rust-lang.org/stable/book/ch12-04-testing-the-librarys-functionality.html

## Detailed Summary

12.3 da `src/lib.rs` ga ajratilgan logika tufayli endi funksiyalarni bevosita test qilish mumkin. Bu bo'lim TDD (Test-Driven Development) yondashuvi bilan `search` funksiyasini qurishni ko'rsatadi.

### TDD jarayoni (4 qadam)

1. **Muvaffaqiyatsiz test yozing** — va u kutilgan sababdan muvaffaqiyatsiz bo'lishini tekshiring
2. **Testni o'tkazadigan minimal kod yozing yoki o'zgartiring**
3. **Refactor qiling** — testlar o'tishda davom etsin
4. **1-qadam dan takrorlang**

### search funksiyasi: TDD bilan qurilishi

**1-qadam: Muvaffaqiyatsiz test**

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn one_result() {
        let query = "duct";
        let contents = "\
Rust:
safe, fast, productive.
Pick three.";

        assert_eq!(vec!["safe, fast, productive."], search(query, contents));
    }
}
```

Avval funksiya `unimplemented!()` bilan e'lon qilinadi — test panic qiladi.

**2-qadam: Compile bo'lishi uchun minimal kod — bo'sh vector**

```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    vec![]
}
```

Endi test compile bo'ladi, lekin assert muvaffaqiyatsiz — kutilgan holat.

**3-qadam: To'liq implementatsiya**

```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();

    for line in contents.lines() {       // har bir satrni iteratsiya
        if line.contains(query) {         // query bormi?
            results.push(line);           // bo'lsa saqla
        }
    }

    results
}
```

Test o'tadi!

### Lifetime `'a` nima uchun kerak?

`search` signaturasida ikkita `&str` parametr bor: `query` va `contents`. Return — `Vec<&'a str>`.

**Muammo**: compiler qaytarilgan string slices qaysi parametrdan kelishini bilmaydi.

**Javob**: `contents` dan — chunki biz fayl mazmunidagi satrlarni qaytaramiz, `query` ni emas.

```rust
// Noto'g'ri — compiler "query yoki contents?" deb so'raydi
pub fn search(query: &str, contents: &str) -> Vec<&str> { ... }
// → E0106: missing lifetime specifier

// To'g'ri — 'a faqat contents va return'ga bog'langan
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> { ... }
```

Lifetime annotatsiya lifetime'ni uzaytirmaydi — faqat `return value` va `contents` ning lifetimini bog'liq ekanini bildiradi.

Kompiler taklif qiladigan tuzatish noto'g'ri bo'lishi mumkin — u `query` va `contents` ga ham `'a` qo'shishni taklif qilishi mumkin. Lekin bu ortiqcha: biz faqat `contents` va return'ni bog'lashimiz kerak.

### `str::lines()` va `str::contains()`

```rust
contents.lines()        // satr iteratori (newline bo'yicha bo'ladi)
line.contains(query)    // substring mavjudligini tekshiradi -> bool
```

### TDD afzalliklari bu loyihada

12.3 da `src/lib.rs` ga ajratish sababli `search` funksiyasini CLI orqali emas, to'g'ridan-to'g'ri test qilish mumkin bo'ldi. Bu TDD ni amaliy qiladi.

## Key Concepts

- [[tdd|Test-Driven Development (TDD)]]
- [[lifetimes|lifetimes]] — `'a` qaysi argument return bilan bog'liqligini bildiradi
- [[testing|testing]]
- [[cfg-test|cfg(test)]]
- [[unit-tests|unit tests]]
- [[iterators|iterators]] — `lines()` iterator qaytaradi
- [[string-slice|string slice]] — qaytarilgan `&'a str`
- [[e0106-missing-lifetime-specifier|E0106]] — lifetime yo'q bo'lganda

## Code Examples

**To'liq `search` funksiyasi (Listing 12-19):**

```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();

    for line in contents.lines() {
        if line.contains(query) {
            results.push(line);
        }
    }

    results
}
```

**Test (Listing 12-15):**

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn one_result() {
        let query = "duct";
        let contents = "\
Rust:
safe, fast, productive.
Pick three.";

        assert_eq!(vec!["safe, fast, productive."], search(query, contents));
    }
}
```

## Exercises or Practice Ideas

- TDD bilan `search_case_insensitive` funksiyasini yozing (12.5 dan oldin o'zingiz sinab ko'ring)
- `search` ga ko'p so'z qidirish imkoniyatini qo'shing
- Hech qanday natija topilmasa haqida test yozing
- Lifetime annotatsiyasini `query` ga ham qo'shsangiz nima bo'ladi?

## Questions Raised

- Nima uchun compiler `'a` ni `query` ga ham qo'yishni taklif qilsa ham, bu noto'g'ri?
- `lines()` iterator qanday ishlaydi? (13-bob)
- Iterator adapters bilan `search` qanday yaxshilanadi? (13-bob)

## Links To Update

- [[tdd]]
- [[lifetimes]]
- [[12-an-io-project]] (chapter)
