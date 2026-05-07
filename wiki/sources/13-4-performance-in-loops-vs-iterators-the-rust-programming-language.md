---
title: "13.4. Performance: Comparing Loops and Iterators"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, performance, iterators, zero-cost-abstractions, benchmark]
source_count: 1
---

# 13.4. Performance: Comparing Loops and Iterators

## Source

- URL: https://doc.rust-lang.org/stable/book/ch13-04-performance.html
- Kitob: The Rust Programming Language
- Bob: 13.4

## Detailed Summary

Bu bo'lim savol'ga javob beradi: **for loop tezroqmi yoki iteratormi?**

### Benchmark natijasi

Sir Arthur Conan Doyle'ning "The Adventures of Sherlock Holmes" kitobida "the" so'zini qidirib benchmark o'lchangan:

```
test bench_search_for  ... bench:  19,620,300 ns/iter (+/- 915,700)
test bench_search_iter ... bench:  19,234,900 ns/iter (+/- 657,200)
```

**Natija: deyarli teng. Iterator biroz tezroq.**

Bu tasodif emas. Compiler iterator chain'ni optimallashtiradi:
- **Loop unrolling** — tsiklni qo'lda "yozilgan" holga keltiradi
- **Bounds check elimination** — massiv indeks tekshiruvlarini olib tashlaydi
- Natijada: `for` loop bilan yozilgan assemblydek assembly chiqadi

### Zero-cost abstractions

[[zero-cost-abstractions]] Rust'ning asosiy dizayn maqsadi. Bjarne Stroustrup (C++ yaratuvchisi, 2012) ta'rifi:

> "In general, C++ implementations obey the zero-overhead principle: What you don't use, you don't pay for. And further: What you do use, you couldn't hand code any better."

Rust bu tamoyilni qabul qilgan. Closures va iterators yuqori darajadagi abstraktsiya, lekin runtime'da qo'l bilan yozilgan past darajali koddan farq qilmaydi.

### Bob xulosasi

> "Closures and iterators are Rust features inspired by functional programming language ideas. They contribute to Rust's capability to clearly express high-level ideas at low-level performance. The implementations of closures and iterators are such that runtime performance is not affected. This is part of Rust's goal to strive to provide zero-cost abstractions."

Qisqacha: closures va iterators dan qo'rqmang — ular tez va idiomatic.

## Key Concepts

- [[zero-cost-abstractions]] — iterators va closures ham ushbu kategoriyaga kiradi (13.4 manbasi qo'shildi)
- [[iterators]] — lazy, zero-overhead
- [[closures]] — zero-overhead
- [[monomorphization]] — generics kabi, iterators ham compile-time specialized bo'ladi

## Code Examples

```
test bench_search_for  ... bench:  19,620,300 ns/iter (+/- 915,700)
test bench_search_iter ... bench:  19,234,900 ns/iter (+/- 657,200)
```

## Exercises or Practice Ideas

- O'zingizning loop va iterator versiyangizni `cargo bench` bilan o'lchab ko'ring
- `criterion` crate bilan mustahkam benchmark yozing

## Questions Raised

- Qachon iterator for-loopdan sekinroq bo'lishi mumkin? (masalan, dynamic dispatch `dyn Iterator`)
- `rayon` crate parallel iteratorlarda qanday performance beradi?

## Links To Update

- [[zero-cost-abstractions]]
- [[13-functional-language-features]] chapter
