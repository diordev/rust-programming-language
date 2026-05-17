---
title: "12.5. Working with Environment Variables"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, environment-variables, minigrep, cli]
source_count: 1
---

# 12.5. Working with Environment Variables

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: 12.5 — Working with Environment Variables
- URL: https://doc.rust-lang.org/stable/book/ch12-05-working-with-environment-variables.html

## Detailed Summary

Bu bo'limda `minigrep` loyihasiga yangi xususiyat qo'shildi: environment variable orqali boshqariladigan case-insensitive qidiruv. Foydalanuvchi `IGNORE_CASE=1` ni terminal sessioniga bir marta o'rnatadi va barcha qidiruvlar katta-kichik harfni hisobga olmaydi.

### TDD jarayoni davom etadi

12.4 bo'limidagi TDD yondashuvi qo'llanildi:

1. **Muvaffaqiyatsiz test yozildi** — `case_insensitive()` test `search_case_insensitive` funksiyasini chaqiradi, lekin bu funksiya hali yo'q
2. **Eski test qayta nomlandi** — `one_result` → `case_sensitive`, ya'ni semantikasi aniqroq bo'ldi
3. **Test mazmuni yangilandi** — `"Duct tape."` satri `case_sensitive` testga qo'shildi (katta D — bu `"duct"` so'roviga mos kelmaydi, regression testini kafolatlaydi)
4. **Implementatsiya yozildi** — testlar o'tdi

### `search_case_insensitive` funksiyasi

```rust
pub fn search_case_insensitive<'a>(
    query: &str,
    contents: &'a str,
) -> Vec<&'a str> {
    let query = query.to_lowercase();
    let mut results = Vec::new();

    for line in contents.lines() {
        if line.to_lowercase().contains(&query) {
            results.push(line);
        }
    }

    results
}
```

**Muhim nuqta:** `query.to_lowercase()` yangi `String` qaytaradi, `&str` emas. Sababi: kichik harfga o'girish yangi ma'lumot hosil qiladi, mavjud ma'lumotga reference emas. Shuning uchun `contains(&query)` da `&` kerak — `contains` `&str` oladi.

Hayot davomiyligi (`'a`) faqat `contents` bilan bog'liq — qaytariladigan satrlar `contents`dan keladi, `query`dan emas.

### `Config` structi kengaytirildi

`ignore_case: bool` maydoni qo'shildi:

```rust
pub struct Config {
    pub query: String,
    pub file_path: String,
    pub ignore_case: bool,
}
```

### `env::var` bilan environment variable o'qish

```rust
let ignore_case = env::var("IGNORE_CASE").is_ok();
```

- `env::var("IGNORE_CASE")` — `Result<String, VarError>` qaytaradi
- O'rnatilgan bo'lsa → `Ok(value)`
- O'rnatilmagan bo'lsa → `Err(VarError::NotPresent)`
- `.is_ok()` — qiymat muhim emas, shunchaki o'rnatilganmi-yo'qmi tekshiradi
- `unwrap()` yoki `expect()` emas — chunki qiymat emas, holat muhim

### `run` funksiyasi yangilandi

```rust
let results = if config.ignore_case {
    search_case_insensitive(&config.query, &contents)
} else {
    search(&config.query, &contents)
};
```

### Foydalanish

```shell
# Case-sensitive (oddiy)
$ cargo run -- to poem.txt

# Case-insensitive (IGNORE_CASE o'rnatilgan)
$ IGNORE_CASE=1 cargo run -- to poem.txt

# PowerShell
PS> $Env:IGNORE_CASE=1; cargo run -- to poem.txt
PS> Remove-Item Env:IGNORE_CASE   # o'chirish
```

### Muhim eslatma

`to_lowercase()` asosiy Unicode bilan ishlaydi, lekin 100% aniq emas. Real ilovada chuqurroq Unicode normalizatsiyasi kerak bo'lishi mumkin.

## Key Concepts

- [[env-var|env::var]] — environment variableni o'qish
- [[tdd|TDD]] — avval test, keyin implementatsiya
- [[lifetimes]] — `'a` faqat `contents` bilan bog'liq
- [[shadowing]] — `let query = query.to_lowercase()` (bir xil nom bilan yangi binding)
- [[result|Result]] — `env::var` `Result` qaytaradi

## Code Examples

- `search_case_insensitive` — `to_lowercase()` bilan case-insensitive qidiruv
- `env::var("IGNORE_CASE").is_ok()` — environment variable holati

## Exercises or Practice Ideas

1. Command line argument VA environment variable orqali case sensitivity boshqarishni amalga oshiring; qaysi biri ustunlik qilishini hal qiling.
2. `IGNORE_CASE` o'rniga boshqa environment variable nomi ishlatib ko'ring.
3. Bir nechta environment variable ni tekshirish uchun `env::vars()` ni o'rganib chiqing.

## Questions Raised

- `env::var` bilan `env::var_os` farqi nima? (OS-native string uchun)
- Bir vaqtda CLI argument ham, env var ham o'rnatilsa, qaysi biri ustunlik qilishi kerak? (Convention: CLI argument odatda ustuvor)

## Links To Update

- [[12-an-io-project]] — 12.5 bo'limini qo'shish
- [[index]] — manba havolasini qo'shish
- [[env-var]] — yangi kontseptsiya sahifasi
