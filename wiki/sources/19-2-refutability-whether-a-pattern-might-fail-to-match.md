---
title: "19.2. Refutability: Whether a Pattern Might Fail to Match"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, patterns, refutability, irrefutable, refutable]
source_count: 1
---

# 19.2. Refutability: Whether a Pattern Might Fail to Match

## Source

- URL: https://doc.rust-lang.org/stable/book/ch19-02-refutability.html
- Raw fayl: `raw/books/the_rust_programming_language/19.2. Refutability Whether a Pattern Might Fail to Match.md`

## Detailed Summary

Patternlar ikki turga bo'linadi:

### Irrefutable Pattern

Har qanday mumkin bo'lgan qiymatga mos keladigan pattern. Masalan `let x = 5;`dagi `x` — bu irrefutable, chunki `x` har qanday qiymatni qabul qiladi va hech qachon mos kelmaslik holati yo'q.

### Refutable Pattern

Ba'zi qiymatlar uchun mos kelmasligi mumkin bo'lgan pattern. Masalan `if let Some(x) = a_value`dagi `Some(x)` — agar `a_value` `None` bo'lsa mos kelmaydi.

### Kontekstga qarab qoidalar

| Konstruksiya | Qabul qiladigan pattern turi |
|---|---|
| `fn` parametrlari | Faqat irrefutable |
| `let` iborasi | Faqat irrefutable |
| `for` loop | Faqat irrefutable |
| `if let` | Ikkisi ham (irrefutable bo'lsa warning) |
| `while let` | Ikkisi ham (irrefutable bo'lsa warning) |
| `let...else` | Ikkisi ham (irrefutable bo'lsa warning) |
| `match` armlar | Oxirgi arm irrefutable bo'lishi kerak |

Funksiya parametrlari, `let` va `for` looplar faqat irrefutable qabul qiladi — chunki qiymat mos kelmasa dastur nima qilishini bilmaydi.

`if let`, `while let`, `let...else` esa refutable pattern kutadi (chunki ular "muvaffaqiyatsizlik" holatini handle qila oladi). Ularga irrefutable pattern berilsa, compiler ogohlantiradi: "shart doim true bo'ladi, `else` kerak emas".

### Amaliy misollar

**Xato: refutable pattern `let`da:**

```rust
let some_option_value: Option<i32> = None;
let Some(x) = some_option_value;  // XATO: E0005
```

Compiler xatosi:
```
error[E0005]: refutable pattern in local binding
  |     let Some(x) = some_option_value;
  |         ^^^^^^^ pattern `None` not covered
  |
  = note: `let` bindings require an "irrefutable pattern"
  help: you might want to use `let else`
  |     let Some(x) = some_option_value else { todo!() };
```

**To'g'ri: `let...else` bilan refutable pattern:**

```rust
let some_option_value: Option<i32> = None;
let Some(x) = some_option_value else {
    return;
};
// x bu yerda ishlatilishi mumkin
```

**Ogohlantirish: irrefutable pattern `let...else`da:**

```rust
let x = 5 else {
    return;
};
// warning: irrefutable `let...else` pattern
// this pattern will always match, so the `else` clause is useless
```

### match va refutability

`match` armlarida odatda refutable patternlar ishlatiladi. Lekin oxirgi arm irrefutable bo'lishi mumkin (va ko'pincha zarur):

```rust
match x {
    Some(i) => println!("{i}"),  // refutable
    None => println!("nothing"), // refutable (lekin bu oxirgi, barcha qolganlarni yopadi)
}
```

Bitta arm bilan `match` yozish mumkin, lekin u oddiy `let`ga teng va kam foydali.

### Xulosa: Amalda qachon muammo bo'ladi

Odatda bu farqni bilish kerak emas — compiler o'zi xato/warning beradi va `let...else`ga o'tishni tavsiya qiladi. Asosiy holat: `Option` yoki `Result` qiymatini `let` bilan ochishga urinish.

## Key Concepts

- [[irrefutable-pattern|irrefutable pattern]]
- [[refutable-pattern|refutable pattern]]
- [[let-else|let...else]]
- [[if-let|if let]]
- [[while-let|while let]]
- [[pattern-matching|pattern matching]]
- [[option|Option]]

## Code Examples

```rust
// Irrefutable
let x = 5;
let (a, b) = (1, 2);

// Refutable (if let bilan to'g'ri)
if let Some(x) = maybe_value { ... }

// Refutable (let...else bilan to'g'ri)
let Some(x) = maybe_value else { return; };

// XATO: refutable let da
// let Some(x) = maybe_value;  // E0005
```

## Exercises or Practice Ideas

- `let Some(x) = value;` ni `let...else` ga o'tkazing.
- `if let` o'rniga `let...else` ishlatib funksiyadan erta chiqishni mashq qiling.
- `match` armlarida irrefutable arm qayerda joylashishi kerakligini tekshiring.

## Questions Raised

- `let...else` bilan `if let` qaysi hollarda ma'qulroq?
- Pattern muvaffaqiyatsiz bo'lganda `let...else` blokidan qanday chiqiladi?

## Links To Update

- [[wiki/chapters/19-2-refutability-whether-a-pattern-might-fail-to-match]]
- [[irrefutable-pattern]] — yangi concept
- [[refutable-pattern]] — yangi concept
- [[let-else]] — 19.2 dan refutability misoli qo'shish
- [[if-let]] — 19.2 dan exhaustiveness yo'qligi haqida qo'shish
