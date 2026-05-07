---
title: "Test-Driven Development (TDD)"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, tdd, design]
source_count: 1
---

# Test-Driven Development (TDD)

## Short Definition

TDD — kodni yozishdan oldin test yoziladigan dasturlash yondashuvi. Test avval muvaffaqiyatsiz bo'ladi, keyin test o'tadigan minimal kod yoziladi, so'ngra refactor qilinadi.

## Why It Matters

- Kod to'g'riligini yuqori saqlaydi (test coverage)
- Funksiya interfeysini oldindan o'ylashga majbur qiladi
- Refactoring xavfsiz — testlar regressiyani ushlaydi

Rust'da `src/lib.rs` logikani ajratib, TDD ni amaliy qiladi: CLI orqali emas, to'g'ridan-to'g'ri funksiyani test qilish mumkin.

## Mental Model

```
[muvaffaqiyatsiz test] → [minimal kod] → [test o'tadi] → [refactor] → [takror]
        RED                  GREEN              GREEN          GREEN
```

## Syntax and Examples

`minigrep` dagi `search` funksiyasi TDD bilan qurilishi:

**1. Muvaffaqiyatsiz test (RED):**

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

// stub — panic qiladi
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    unimplemented!()
}
```

**2. Compile bo'lsin, lekin fail bo'lsin (bo'sh vector):**

```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    vec![]
}
```

**3. To'liq implementatsiya (GREEN):**

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

**4. Refactor** — testlar o'tishda davom etsa, iteratorlar bilan yaxshilash mumkin (13-bob).

## TDD 4 qadami

1. **Muvaffaqiyatsiz test yozing** — kutilgan xulq-atvorni ifodalaydi
2. **Testni o'tkazadigan minimal kod yozing** — ortiqcha emas
3. **Refactor qiling** — testlar yashil holda qolsin
4. **Takrorlang**

## Common Mistakes

- `unimplemented!()` o'rniga `todo!()` ham ishlatilishi mumkin — ikkalasi ham panic
- TDD da test oldin yoziladi — implementatsiyadan oldin
- "Yashil" holat eng sodda kod bo'lishi kerak, ortiqcha logika emas

## Related Concepts

- [[testing|testing]]
- [[unit-tests|unit tests]]
- [[cfg-test|cfg(test)]]
- [[test-macros|test macros]]
- [[separation-of-concerns|separation of concerns]] — lib.rs ajratish TDD ni osonlashtiradi
- [[refactoring|refactoring]]

## Sources

- [[12-4-adding-functionality-with-test-driven-development-the-rust-programming-language]]
