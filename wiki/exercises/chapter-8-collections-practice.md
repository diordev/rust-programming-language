---
title: "Chapter 8 Collections Practice"
type: exercise
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, exercise, collections]
source_count: 1
---

# Chapter 8 Collections Practice

## Prompt

Chapter 8 yakunidagi uch mashq [[vector|Vec<T>]], [[string-type|String]], [[string-slice|String slice]], va [[hash-map|HashMap]]ni birga qo'llashni mashq qildiradi.

## Exercises

1. Integer list berilganda median va mode toping. Median uchun vectorni sort qiling; mode uchun [[hash-map|HashMap]] word-countga o'xshash count patternidan foydalaning.
2. Stringlarni Pig Latin'ga convert qiling. Birinchi consonant word oxiriga o'tadi va `ay` qo'shiladi; vowel bilan boshlangan word oxiriga `hay` qo'shiladi. [[utf-8|UTF-8]] detailsni yodda tuting.
3. `HashMap` va vectors bilan employee department interface yarating. Masalan, `Add Sally to Engineering` yoki `Add Amir to Sales`; keyin department bo'yicha yoki company bo'yicha sorted employees ro'yxatini qaytaring.

## Review Notes

- Median/mode mashqi `Vec<T>` sorting va `HashMap` countingni birlashtiradi.
- Pig Latin mashqi string slicing va Unicode assumptionsni ehtiyotkorlik bilan ko'rib chiqishni talab qiladi.
- Employee department mashqi `HashMap<String, Vec<String>>` kabi nested collection patterniga olib boradi.

## Related Pages

- [[wiki/chapters/8-common-collections|8. Common Collections]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[string-type|String]]
- [[utf-8|UTF-8]]
- [[word-count-hashmap|word count hashmap]]
