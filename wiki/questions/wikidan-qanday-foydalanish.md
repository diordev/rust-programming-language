---
title: "Wikidan qanday foydalanish mumkin?"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, wiki, workflow]
source_count: 0
---

# Wikidan qanday foydalanish mumkin?

## Answer

Bu wiki ikki tomonlama ishlaydi: user uchun durable learning reference, LLM uchun esa persistent context.

User sifatida foydalanish:

- [[index|Rust Wiki Index]]dan katalog sifatida boshlash.
- [[overview|Rust Wiki Overview]]dan hozirgi umumiy synthesisni o'qish.
- [[rust-learning-roadmap]] orqali qaysi mavzular o'rganilganini va keyingi mavzularni ko'rish.
- Concept sahifalarni Obsidian wikilinks orqali yurib o'qish: masalan [[ownership]], [[borrowing]], [[structs]], [[methods]].
- Source summarylardan raw source'ni qayta o'qimasdan chapter mazmunini eslash.
- Error sahifalaridan compiler diagnostics sabab va fix patternlarini topish.
- Examples, exercises, comparisons, patterns sahifalaridan practice va review uchun foydalanish.

LLM uchun foydalanish:

- Savol berilganda LLM avval [[index|Rust Wiki Index]]ni, keyin relevant concept/chapter/source/question sahifalarni o'qiydi.
- Javob raw source'ni har safar boshidan qayta summarize qilishga emas, wiki'dagi curated synthesisga tayanadi.
- Yangi manba ingest qilinganda source summary, concept pages, overview, index, log, va ontology asta-sekin yangilanadi.
- Yaxshi savol-javoblar keyin qayta ishlatish uchun `wiki/questions/`ga saqlanadi.

Eng yaxshi workflow:

1. Yangi materialni `raw/books/`, `raw/articles/`, yoki mos raw folderga qo'ying.
2. "ingest qiling" deb ayting.
3. LLM key takeawaysni ko'rsatadi; tasdiqdan keyin wiki'ni yangilaydi.
4. Keyin Rust savollarini oddiy tilda bering: "ownership bilan borrowing farqi nima?", "E0277 nima?", "struct update syntaxda move qanday ishlaydi?"
5. LLM wiki sahifalariga tayanib javob beradi va kerak bo'lsa durable javobni saqlaydi.

Qisqa qilib: bu wiki faqat LLM qidirishi uchun emas. Siz uchun o'qiladigan Obsidian knowledge base, men uchun esa har safar yaxshiroq va izchil javob berishga yordam beradigan long-term memory.

## Evidence

- [[index|Rust Wiki Index]]
- [[overview|Rust Wiki Overview]]
- [[rust-learning-roadmap]]
- [[log|Wiki Log]]

## Follow-up

- Obsidian'da `wiki/index.md`ni home note sifatida pin qilish.
- Haftada bir marta "lint qiling" deb orphan links, missing summaries, stale pages va review itemsni tekshirtirish.
- Har bir katta chapterdan keyin "review savollar tuzing" deb `wiki/exercises/`ni boyitish.

## Related Pages

- [[index|Rust Wiki Index]]
- [[overview|Rust Wiki Overview]]
- [[rust-learning-roadmap]]
- [[log|Wiki Log]]
