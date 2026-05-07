---
title: "unwrap_or_else"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, result, option, closure, error-handling]
source_count: 1
---

# unwrap_or_else

## Short Definition

`Result<T, E>` va `Option<T>` dagi metod: muvaffaqiyatda ichki qiymatni qaytaradi, xato/`None` bo'lsa berilgan closure'ni chaqiradi.

## Why It Matters

`unwrap()` xato bo'lsa panic qiladi — bu foydalanuvchi uchun yoqimsiz. `unwrap_or_else` esa xatoni o'zimiz istagancha ishlashga imkon beradi: xato xabari chiqarish, dasturni tozaroq to'xtatish va hokazo.

## Mental Model

```
Result::Ok(val)  → val qaytaradi (closure chaqirilmaydi)
Result::Err(e)   → closure(e) chaqiriladi
```

## Syntax and Examples

```rust
let config = Config::build(&args).unwrap_or_else(|err| {
    println!("Problem parsing arguments: {err}");
    process::exit(1);
});
```

- `|err|` — closure parametri: `Err` ichidagi qiymat
- `process::exit(1)` — dasturni to'xtatadi (closure hech qachon `Config` qaytarmaydi, lekin bu OK — exit qilindi)

### `Option` bilan:

```rust
let val = some_option.unwrap_or_else(|| {
    eprintln!("Value was missing!");
    default_value()
});
```

### `unwrap_or` bilan farqi:

| | `unwrap_or` | `unwrap_or_else` |
|---|---|---|
| Default | Statik qiymat | Closure (lazy) |
| Qachon hisoblash | Har doim | Faqat `Err`/`None` bo'lsa |
| Qiymat | `T` | `FnOnce() -> T` |

Katta yoki tez hisoblash talab qiladigan default uchun `unwrap_or_else` yaxshiroq.

## Common Mistakes

- Closure ichida return turini noto'g'ri tushunish — closure `T` qaytarishi yoki diverge qilishi kerak (`process::exit`, `panic!`)
- `unwrap_or` bilan chalkashish — static qiymat uchun `unwrap_or`, dinamik yoki closure uchun `unwrap_or_else`

## Related Concepts

- [[result|Result]]
- [[option|Option]]
- [[unwrap|unwrap]]
- [[expect|expect]]
- [[closures|closures]]
- [[process-exit|process::exit]]
- [[error-handling|error handling]]

## Sources

- [[12-3-refactoring-to-improve-modularity-and-error-handling-the-rust-programming-language]]
