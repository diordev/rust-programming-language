---
title: "Wiki Log"
type: overview
status: active
created: 2026-05-06
updated: 2026-05-17
tags: [rust, log]
source_count: 146
---

# Wiki Log

## 2026-05-17 ingest | Rust for Backend Developers `2. base` 33-35

- Ingested 3 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `33. Перечисления.md`
  - `34. Option.md`
  - `35. Result.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-enums]]
  - [[wiki/sources/rust-for-backend-developers-option]]
  - [[wiki/sources/rust-for-backend-developers-result]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-enums]]
  - [[wiki/chapters/rust-for-backend-developers-option]]
  - [[wiki/chapters/rust-for-backend-developers-result]]
- Created concept page:
  - [[std-error-trait|std::error::Error]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 25->28; enums, enum payloads, discriminant mental modeli, [[if-let|if let]], [[option|Option]], [[result|Result]], combinatorlar, [[std-error-trait|std::error::Error]], [[question-mark-operator|question mark operator]], va ignored `Result` synthesis qo'shildi.
- Updated concept pages: [[enums]], [[enum-variants|enum variants]], [[generic-enums|generic enums]], [[match]], [[if-let|if let]], [[option|Option]], [[unwrap]], [[closures]], [[result|Result]], [[error-handling]], [[recoverable-errors|recoverable errors]], [[question-mark-operator|question mark operator]], [[error-propagation|error propagation]], va [[discarded-binding]].
- Updated [[index|Rust Wiki Index]] va [[overview|Rust Wiki Overview]] source_count 143->146; 3 source, 3 chapter, va 1 concept link qo'shildi.
- Appended enums/option/result relations to `infranodus/rust-book-relations.txt`; ontology qayta generatsiya qilinmadi.
- Source caveat preservation: enum memory layout taxminiy yozildi; [[unwrap]] `unsafe` deb emas, panic qilishi mumkin bo'lgan shortcut sifatida yozildi; `?` errorni handle qilmasdan propagate qilishi ochiq aytildi; [[std-error-trait|std::error::Error]] qo'lda implement qilinishi mumkinligi, lekin helper crate/macrolar amalda ko'p ishlatilishi qayd etildi.

## 2026-05-17 ingest | Rust for Backend Developers `2. base` 32

- Ingested 1 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `32. Генерики.md`
- Created source summary:
  - [[wiki/sources/rust-for-backend-developers-generics]]
- Created chapter page:
  - [[wiki/chapters/rust-for-backend-developers-generics]]
- Created concept page:
  - [[const-generics|const generics]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 24->25; `Holder<T>`, generic functions, generic `impl`, [[monomorphization]], [[turbofish]], generic traits vs [[associated-types|associated types]], [[trait-bounds|trait bounds]], [[where-clauses|where clauses]], conditional impl, va [[const-generics|const generics]] synthesis qo'shildi.
- Updated concept pages: [[generics]], [[generic-type-parameters|generic type parameters]], [[generic-functions|generic functions]], [[generic-structs|generic structs]], [[generic-methods|generic methods]], [[monomorphization]], [[turbofish]], [[trait-bounds|trait bounds]], [[where-clauses|where clauses]], [[impl-trait|impl Trait]], [[associated-types|associated types]], [[traits]], [[function-pointers|function pointers]], va [[fn-traits|Fn traits]].
- Updated [[index|Rust Wiki Index]] va [[overview|Rust Wiki Overview]] source_count 142->143; 1 source, 1 chapter, va 1 concept link qo'shildi.
- Appended generics relations to `infranodus/rust-book-relations.txt`; ontology qayta generatsiya qilinmadi.
- Source caveat preservation: Java/C# type erasure comparison beginner contrast sifatida cheklab yozildi; associated type implementor tanlaydigan bitta bog'langan type ekani ochiq aytildi; `make_array<T: Copy, const SIZE: usize>` va `[T; SIZE]` const-generics uchun asosiy example sifatida saqlandi.

## 2026-05-17 ingest | Rust for Backend Developers `2. base` 28-31

- Ingested 4 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `28. Авто-реализация трэйтов.md`
  - `29. Деструктурирование.md`
  - `30. Паттерн матчинг.md`
  - `31. Анонимные функции.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
  - [[wiki/sources/rust-for-backend-developers-destructuring]]
  - [[wiki/sources/rust-for-backend-developers-pattern-matching]]
  - [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-auto-derive-traits]]
  - [[wiki/chapters/rust-for-backend-developers-destructuring]]
  - [[wiki/chapters/rust-for-backend-developers-pattern-matching]]
  - [[wiki/chapters/rust-for-backend-developers-anonymous-functions]]
- Created concept pages:
  - [[attribute]]
  - [[marker-trait|marker trait]]
  - [[array-destructuring|array destructuring]]
  - [[slice-patterns|slice patterns]]
  - [[ref-pattern|ref pattern]]
  - [[higher-order-functions|higher-order functions]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 20->24; derive, Clone, Copy, destructuring, `match`, closure, function pointer, `Fn`/`FnMut`/`FnOnce`, va `impl Fn`/`Box<dyn Fn>` synthesis qo'shildi.
- Updated concept pages: [[derive-attribute|derive attribute]], [[partial-eq|PartialEq]], [[clone]], [[copy-trait|Copy trait]], [[debug-trait|Debug trait]], [[default-trait|Default trait]], [[hash-trait|Hash trait]], [[pattern-destructuring|pattern destructuring]], [[match]], [[pattern-matching|pattern matching]], [[match-guard|match guard]], [[at-binding|@ binding]], [[discarded-binding]], [[closures]], [[function-pointers|function pointers]], [[fn-traits|Fn traits]], [[returning-closures|returning closures]], [[opaque-types|opaque types]], [[move-semantics|move semantics]].
- Updated [[index|Rust Wiki Index]] va [[overview|Rust Wiki Overview]] source_count 138->142; 4 source, 4 chapter, va 6 concept link qo'shildi.
- Appended derive/destructuring/pattern matching/anonymous-functions relations to `infranodus/rust-book-relations.txt`; ontology qayta generatsiya qilinmadi.
- Source caveat preservation: `Copy` semantics literal `.clone()` call deb emas, implicit copy modeli sifatida yozildi; `Fn`/`FnMut`/`FnOnce` thread-safety kategoriyasi sifatida emas, callability/capture behavior sifatida tushuntirildi; `impl Fn` bitta concrete closure type ekanligi va turli branchlar uchun `Box<dyn Fn>` kerak bo'lishi ochiq yozildi.

## 2026-05-08 ingest | 19.3. Pattern Syntax

- Ingested `raw/books/the_rust_programming_language/19.3. Pattern Syntax.md`.
- Created source summary [[wiki/sources/19-3-pattern-syntax]].
- Created chapter page [[wiki/chapters/19-3-pattern-syntax]].
- Created concept pages [[match-guard|match guard]], [[at-binding|@ binding]], [[or-pattern|or-pattern (`|`)]].
- Updated concept pages [[pattern-matching|pattern matching]] (source_count 7→8), [[pattern-destructuring|pattern destructuring]] (source_count 1→2), [[catch-all-patterns|catch-all patterns]] (source_count 1→2): `..`, `_name` tafsilotlari qo'shildi.
- Updated [[index|Rust Wiki Index]]: +1 source, +1 chapter, +3 concept; source_count 92→93.
- Updated [[overview|Rust Wiki Overview]]: 19.3 synthesis; source_count 83→84.
- Updated `infranodus/rust-book-relations.txt`: 12 ta yangi relatsiya.

## 2026-05-08 ingest | 19. Patterns and Matching (19.0, 19.1, 19.2)

- Ingested `raw/books/the_rust_programming_language/19. Patterns and Matching.md`.
- Ingested `raw/books/the_rust_programming_language/19.1. All the Places Patterns Can Be Used.md`.
- Ingested `raw/books/the_rust_programming_language/19.2. Refutability Whether a Pattern Might Fail to Match.md`.
- Created source summaries [[wiki/sources/19-patterns-and-matching]], [[wiki/sources/19-1-all-the-places-patterns-can-be-used]], [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match]].
- Created chapter pages [[wiki/chapters/19-patterns-and-matching]], [[wiki/chapters/19-1-all-the-places-patterns-can-be-used]], [[wiki/chapters/19-2-refutability-whether-a-pattern-might-fail-to-match]].
- Created concept pages [[while-let|while let]], [[irrefutable-pattern|irrefutable pattern]], [[refutable-pattern|refutable pattern]], [[pattern-destructuring|pattern destructuring]].
- Updated concept pages [[pattern-matching|pattern matching]] (source_count 4→7), [[if-let|if let]] (source_count 1→2), [[match]] (source_count 4→5).
- Updated [[index|Rust Wiki Index]]: +3 source, +3 chapter, +4 concept; source_count 89→92.
- Updated `infranodus/rust-book-relations.txt`: 14 ta yangi relatsiya.

## 2026-05-08 ingest | 18.3. Implementing an Object-Oriented Design Pattern

- Ingested `raw/books/the_rust_programming_language/18.3. Implementing an Object-Oriented Design Pattern.md`.
- Created source summary [[wiki/sources/18-3-implementing-an-object-oriented-design-pattern]].
- Created chapter page [[wiki/chapters/18-3-implementing-an-object-oriented-design-pattern]].
- Created concept pages [[state-pattern|State design pattern]] va [[typestate-pattern|Typestate pattern]].
- Created example pages [[blog-post-state-pattern]] va [[blog-post-typestate]].
- Updated [[index|Rust Wiki Index]]: +1 source, +1 chapter, +2 concept, +2 example; source_count 88→89.
- Updated [[overview|Rust Wiki Overview]]: 18.3 synthesis; source_count 79→80.
- Updated `infranodus/rust-book-relations.txt`: 6 ta yangi relatsiya.

## 2026-05-08 ingest | 18. Object Oriented Programming Features (18.0, 18.1, 18.2)

- Ingested `raw/books/the_rust_programming_language/18. Object Oriented Programming Features.md`.
- Ingested `raw/books/the_rust_programming_language/18.1. Characteristics of Object-Oriented Languages.md`.
- Ingested `raw/books/the_rust_programming_language/18.2. Using Trait Objects to Abstract over Shared Behavior.md`.
- Created source summaries [[18-object-oriented-programming-features]], [[wiki/sources/18-1-characteristics-of-object-oriented-languages]], [[18-2-using-trait-objects-to-abstract-over-shared-behavior]].
- Created chapter pages [[18-object-oriented-programming]], [[wiki/chapters/18-1-characteristics-of-object-oriented-languages]], [[18-2-using-trait-objects]].
- Created concept pages [[trait-object]], [[dynamic-dispatch]], [[polymorphism]], [[encapsulation]].
- Created example page [[gui-draw-trait]].
- Updated [[index|Rust Wiki Index]]: +3 source, +3 chapter, +4 concept, +1 example; source_count 85→88.
- Updated [[overview|Rust Wiki Overview]]: Chapter 18 synthesis; source_count 76→79.
- Updated `infranodus/rust-book-relations.txt`: 8 ta yangi relatsiya.

## 2026-05-08 ingest | 17.5. A Closer Look at the Traits for Async + 17.6. Futures, Tasks, and Threads

- Ingested `raw/books/the_rust_programming_language/17.5. A Closer Look at the Traits for Async.md`.
- Ingested `raw/books/the_rust_programming_language/17.6. Futures, Tasks, and Threads.md`.
- Created source summaries [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async]] va [[wiki/sources/17-6-futures-tasks-and-threads]].
- Created chapter pages [[wiki/chapters/17-5-a-closer-look-at-the-traits-for-async]] va [[wiki/chapters/17-6-futures-tasks-and-threads]].
- Created concept pages [[pin|Pin va Unpin]] va [[join-all]].
- Updated concept pages [[future|Future trait]] (source_count 1→2) va [[stream|Stream]] (source_count 1→2): Pin/poll_next tafsilotlari qo'shildi.
- Created example pages [[join-all-pinned]] va [[thread-async-combined]].
- Updated [[index|Rust Wiki Index]]: +2 source, +2 chapter, +2 concept, +2 example; source_count 83→85.
- Updated [[overview|Rust Wiki Overview]]: 17.5 va 17.6 synthesis; source_count 74→76.
- Updated `infranodus/rust-book-relations.txt`: 9 ta yangi relatsiya.

## 2026-05-08 ingest | 17.3. Working With Any Number of Futures + 17.4. Streams (qisman)

- Ingested `raw/books/the_rust_programming_language/17.3. Working With Any Number of Futures.md`.
- Ingested `raw/books/the_rust_programming_language/17.4. Streams Futures in Sequence.md` (qisman — 91 satr, web clipper to'liq olmagan).
- Created source summaries [[wiki/sources/17-3-working-with-any-number-of-futures]] va [[wiki/sources/17-4-streams-futures-in-sequence]] (needs-review).
- Created chapter pages [[wiki/chapters/17-3-working-with-any-number-of-futures]] va [[wiki/chapters/17-4-streams-futures-in-sequence]] (needs-review).
- Created concept pages [[yield-now]] va [[stream]] (needs-review).
- Created example pages [[async-timeout]] va [[stream-from-iter]].
- Updated [[index|Rust Wiki Index]]: +2 source, +2 chapter, +2 concept, +2 example; source_count 81→83.
- Updated [[overview|Rust Wiki Overview]]: 17.3 va 17.4 synthesis; source_count 72→74.
- Updated `infranodus/rust-book-relations.txt`: 8 ta yangi relatsiya.
- **Flag**: 17.4 raw fayl to'liq emas — to'liq bo'lgach qayta ingest kerak.

## 2026-05-08 ingest | 17.2. Applying Concurrency with Async

- Ingested `raw/books/the_rust_programming_language/17.2. Applying Concurrency with Async.md`.
- Created source summary [[wiki/sources/17-2-applying-concurrency-with-async]].
- Created chapter page [[wiki/chapters/17-2-applying-concurrency-with-async]].
- Created concept pages [[async-task]], [[async-join]], [[async-channel]], [[async-move-block]].
- Created example pages [[async-concurrent-tasks]] va [[async-channel-messages]].
- Updated [[index|Rust Wiki Index]]: yangi source, chapter, 4 concept, 2 example; source_count 80→81.
- Updated [[overview|Rust Wiki Overview]]: 17.2 synthesis qo'shildi, source_count 71→72.
- Updated `infranodus/rust-book-relations.txt`: 10 ta yangi relatsiya qo'shildi.

## 2026-05-08 ingest | 17. Async Programming + 17.1. Futures and the Async Syntax

- Ingested `raw/books/the_rust_programming_language/17. Fundamentals of Asynchronous Programming Async, Await, Futures, and Streams.md`.
- Ingested `raw/books/the_rust_programming_language/17.1. Futures and the Async Syntax.md`.
- Created source summaries [[17-fundamentals-of-asynchronous-programming]] va [[17-1-futures-and-the-async-syntax]].
- Created chapter pages [[17-async-programming]] va [[17-1-futures-and-async-syntax]].
- Created concept pages [[future|Future trait]], [[async-runtime]], [[polling]], [[async-state-machine]], [[cpu-bound]], [[io-bound]].
- Updated concept page [[async-await|async/await]]: stub'dan to'liq sahifaga, status `needs-review` → `active`, misollar qo'shildi, source_count 1→3.
- Updated concept page [[concurrency]]: async muqobil qo'shildi, source_count 6→7.
- Created example page [[async-page-scraper]].
- Updated [[index|Rust Wiki Index]]: yangi sources, chapters, concepts, example; source_count 78→80.
- Updated [[overview|Rust Wiki Overview]]: Chapter 17 synthesis qo'shildi, source_count 69→71.
- Updated `infranodus/rust-book-relations.txt`: 9 ta yangi relatsiya qo'shildi.

## 2026-05-08 ingest | 15.6. Reference Cycles Can Leak Memory

- Ingested `raw/books/the_rust_programming_language/15.6. Reference Cycles Can Leak Memory.md`.
- Created source summary [[15-6-reference-cycles-can-leak-memory]].
- Created concept pages [[weak-t|Weak<T>]], [[reference-cycle]], [[memory-leak]].
- Created example page [[tree-with-weak-parent]].
- Updated [[rc-t|Rc<T>]] concept: Weak<T> va cycle xavfi bo'limlari qo'shildi; source_count 1→2.
- Updated [[wiki/chapters/15-smart-pointers|15. Smart Pointers]]: 15.6 bo'limi (yakuniy summary jadvali bilan), +3 mashq, +4 review savol qo'shildi; source_count 6→7.
- Updated [[index|Rust Wiki Index]]: yangi source; source_count 72→73.
- Updated `infranodus/rust-book-relations.txt`: 13 ta yangi relatsiya qo'shildi.

## 2026-05-08 ingest | 15.4–15.5. Rc<T> va RefCell<T>

- Ingested `raw/books/the_rust_programming_language/15.4. Rc<T>, the Reference Counted Smart Pointer.md`.
- Ingested `raw/books/the_rust_programming_language/15.5. RefCell<T> and the Interior Mutability Pattern.md`.
- Created source summaries [[15-4-rc-the-reference-counted-smart-pointer]] va [[15-5-refcell-t-and-the-interior-mutability-pattern]].
- Created concept pages [[rc-t|Rc<T>]], [[refcell-t|RefCell<T>]], [[interior-mutability]].
- Created example page [[rc-shared-list]].
- Updated [[wiki/chapters/15-smart-pointers|15. Smart Pointers]]: 15.4, 15.5 bo'limlari, +5 mashq, +5 review savol qo'shildi.
- Updated [[index|Rust Wiki Index]]: 2 yangi source; source_count 70 → 72.
- Updated `infranodus/rust-book-relations.txt`: 21 ta yangi relatsiya qo'shildi.

## 2026-05-08 ingest | 15.3. Running Code on Cleanup with the Drop Trait

- Ingested `raw/books/the_rust_programming_language/15.3. Running Code on Cleanup with the Drop Trait.md`.
- Created source summary [[15-3-running-code-on-cleanup-with-the-drop-trait]] in `wiki/sources/`.
- Created error page [[e0040-explicit-use-of-destructor|E0040: explicit use of destructor method]] in `wiki/errors/`.
- Updated [[drop]] concept page: Drop trait implement qilish, E0040 sababi, `std::mem::drop` bo'limlari qo'shildi.
- Updated [[wiki/chapters/15-smart-pointers|15. Smart Pointers]]: 15.3 bo'limi, 3 ta yangi mashq, 3 ta yangi review savol qo'shildi.
- Updated [[index|Rust Wiki Index]]: yangi source qo'shildi; source_count 69 → 70.
- Updated `infranodus/rust-book-relations.txt`: 13 ta yangi relatsiya qo'shildi.

## 2026-05-08 lint | Wiki lint tekshiruvi

- Lint o'tkazildi: 399 sahifa tekshirildi.
- Natija: broken wikilinks 0, orphan 0, frontmatter muammolari 0.
- Topilgan muammo: `raw/books/the_rust_programming_language/15.3. Running Code on Cleanup with the Drop Trait.md` ingest qilinmagan.
- Hisobot saqlandi: `output/2026-05-08-wiki-lint-report.md`.

## 2026-05-08 maintenance + ingest | Rust Book source rename and Chapter 15.0-15.2

- Renamed 66 ta raw Rust Book source file: filenames ichidan takroriy ` - The Rust Programming Language` suffix olib tashlandi.
- Renamed 66 ta `wiki/sources/` summary file: source sluglar raw filename stemiga mos suffixsiz shaklga o'tkazildi.
- Updated wiki, output, todos, va infranodus ichidagi eski source wikilink/path references: source links endi `sources/...`, chapter links esa kerak joyda `chapters/...`.
- Ingested `raw/books/the_rust_programming_language/15. Smart Pointers.md`.
- Ingested `raw/books/the_rust_programming_language/15.1. Using Box<T> to Point to Data on the Heap.md`.
- Ingested `raw/books/the_rust_programming_language/15.2. Treating Smart Pointers Like Regular References.md`.
- Created source summaries [[wiki/sources/15-smart-pointers]], [[15-1-using-box-t-to-point-to-data-on-the-heap]], and [[15-2-treating-smart-pointers-like-regular-references]] in `wiki/sources/`.
- Created chapter page [[wiki/chapters/15-smart-pointers|15. Smart Pointers]].
- Created concept pages [[box-t|Box<T>]], [[recursive-types]], [[deref-trait|Deref trait]], and [[deref-mut-trait|DerefMut trait]].
- Moved [[smart-pointers]] from glossary stub to full concept page in `wiki/concepts/`.
- Created example pages [[box-cons-list]] and [[mybox-deref]].
- Created compiler error pages [[e0072-recursive-type-has-infinite-size|E0072 recursive type has infinite size]] and [[e0614-type-cannot-be-dereferenced|E0614 type cannot be dereferenced]].
- Updated [[deref-coercions]], [[dereference-operator]], [[drop]], [[stack-and-heap]], and [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]] with Chapter 15 context.
- Updated [[index|Rust Wiki Index]] and [[overview|Rust Wiki Overview]]. Current ingested Rust Book source count: 69.
- Saved ingest report to `output/2026-05-08-chapter-15-0-15-2-ingest-report.md`.

## 2026-05-07 ingest | Rust Book 14.0-14.5

- Ingested 6 ta manba: 14.0 (intro), 14.1 (release profiles), 14.2 (publishing), 14.3 (workspaces), 14.4 (cargo install), 14.5 (custom commands).
- Created [[wiki/sources/14-more-about-cargo-and-crates-io]] in `wiki/sources/`.
- Created [[14-1-customizing-builds-with-release-profiles]] in `wiki/sources/`.
- Created [[14-2-publishing-a-crate-to-crates-io]] in `wiki/sources/`.
- Created [[14-3-cargo-workspaces]] in `wiki/sources/`.
- Created [[14-4-installing-binaries-with-cargo-install]] in `wiki/sources/`.
- Created [[14-5-extending-cargo-with-custom-commands]] in `wiki/sources/`.
- Created [[wiki/chapters/14-more-about-cargo-and-crates-io]] chapter page in `wiki/chapters/`.
- Created [[cargo-workspaces]] concept page in `wiki/concepts/`.
- Updated [[documentation-comments]]: `///` vs `//!`, standart bo'limlar, doc-tests, `cargo doc`; status: needs-review → active.
- Updated [[release-build]]: release profiles, `opt-level` jadvali, profil sozlash qo'shildi.
- Updated [[crates-io]]: to'liq publish workflow, `cargo yank`, metadata talablari qo'shildi; status: draft → active.
- Updated [[cargo]]: workspace, `cargo install`, custom subcommands, yangi buyruqlar qo'shildi. source_count: 4 → 7.
- Updated `wiki/index.md`: 6 yangi manba, 1 yangi bob, 1 yangi konsept. source_count: 60 → 66.
- Updated `wiki/overview.md`: 14-bob synthesis qo'shildi. source_count: 53 → 59.
- Chapter 14 to'liq qoplandi (14.0–14.5).

## 2026-05-07 ingest | Rust Book 13.3 va 13.4

- Ingested `raw/books/the_rust_programming_language/13.3. Improving Our IO Project.md`.
- Ingested `raw/books/the_rust_programming_language/13.4. Performance in Loops vs. Iterators.md`.
- Created [[13-3-improving-our-io-project]] in `wiki/sources/`.
- Created [[13-4-performance-in-loops-vs-iterators]] in `wiki/sources/`.
- Created [[minigrep-iterator-refactor]] example page in `wiki/examples/`.
- Updated [[13-functional-language-features]] chapter page: 13.3 va 13.4 bo'limlari, 2 ta yangi exercise va review question qo'shildi.
- Updated [[zero-cost-abstractions]] concept: iterators benchmark ma'lumoti va Stroustrup qoidasi qo'shildi, ortiqcha duplicate link tozalandi.
- Updated `wiki/index.md`: 2 yangi manba, 1 yangi example. source_count: 58 → 60.
- Updated `wiki/overview.md`: 13-bob synthesis to'ldirildi. source_count: 51 → 53.
- Chapter 13 to'liq qoplandi (13.0–13.4).

## 2026-05-07 ingest | Rust Book 13.0, 13.1, 13.2

- Ingested `raw/books/the_rust_programming_language/13. Functional Language Features Iterators and Closures.md`.
- Ingested `raw/books/the_rust_programming_language/13.1. Closures.md`.
- Ingested `raw/books/the_rust_programming_language/13.2. Processing a Series of Items with Iterators.md`.
- Created [[13-functional-language-features-iterators-and-closures]] in `wiki/sources/`.
- Created [[13-1-closures]] in `wiki/sources/`.
- Created [[13-2-processing-a-series-of-items-with-iterators]] in `wiki/sources/`.
- Created [[13-functional-language-features]] chapter page in `wiki/chapters/`.
- Created concept pages: [[closures]], [[fn-traits]], [[iterators]], [[consuming-adapters]], [[iterator-adapters]].
- Updated `wiki/index.md`: 3 yangi manba, 1 yangi bob, 5 yangi konsept. source_count: 55 → 58.
- Updated `wiki/overview.md`: 13-bob synthesis qo'shildi.
- 13-bob qisman qoplandi (13.0, 13.1, 13.2). 13.3 va 13.4 hali ingest qilinmagan.

## 2026-05-07 ingest | Rust Book 12.5 va 12.6

- Ingested `raw/books/the_rust_programming_language/12.5. Working with Environment Variables.md`.
- Ingested `raw/books/the_rust_programming_language/12.6. Redirecting Errors to Standard Error.md`.
- Created [[12-5-working-with-environment-variables]] in `wiki/sources/`.
- Created [[12-6-redirecting-errors-to-standard-error]] in `wiki/sources/`.
- Created concept pages: [[env-var]] (`env::var`), [[eprintln-macro]] (`eprintln!`).
- Updated [[12-an-io-project]] chapter page with 12.5 and 12.6 sections.
- Updated `wiki/index.md`: 2 yangi manba, 2 yangi kontseptsiya.
- 12-bob (`minigrep`) endi to'liq qoplandi.

## 2026-05-06 init | Wiki created

- Created the initial Rust learning wiki scaffold.
- Chosen mode: Heavy tier, weekly work, interactive per-source ingest.
- Language convention: Uzbek explanations with English Rust terms.
- Added folders for concepts, examples, exercises, questions, projects, snippets, errors, crates, tools, and roadmap.

## 2026-05-06 ingest | Rust Book title page

- Ingested `raw/books/the_rust_programming_language/0. The Rust Programming Language.md`.
- Created [[0-the-rust-programming-language]] in `wiki/sources/`.
- Updated `wiki/index.md` and `wiki/overview.md`.
- Recorded baseline assumptions: Rust `1.90.0` or later and Rust 2024 Edition via `edition = "2024"`.

## 2026-05-06 ingest | Rust Book foreword and introduction

- Ingested `raw/books/the_rust_programming_language/0.1. Foreword.md`.
- Ingested `raw/books/the_rust_programming_language/0.2. Introduction.md`.
- Created [[0-1-foreword]] and [[wiki/sources/0-2-introduction]] in `wiki/sources/`.
- Created chapter page [[wiki/chapters/0-2-introduction|0.2. Introduction]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended new Rust Book relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 1

- Ingested `raw/books/the_rust_programming_language/1. Getting Started.md`.
- Ingested `raw/books/the_rust_programming_language/1.1. Installation.md`.
- Ingested `raw/books/the_rust_programming_language/1.2. Hello, World!.md`.
- Ingested `raw/books/the_rust_programming_language/1.3. Hello, Cargo!.md`.
- Created source summaries [[wiki/sources/1-getting-started]], [[1-1-installation]], [[1-2-hello-world]], and [[1-3-hello-cargo]].
- Created chapter page [[wiki/chapters/1-getting-started|1. Getting Started]], example page [[hello-world]], and tool pages [[rustup]], [[rustc]], and [[cargo|Cargo]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 1 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 maintenance | Numbered Rust Book source order

- Renamed Rust Book source summary files in `wiki/sources/` to match the numbered raw source order.
- Renamed chapter pages to [[wiki/chapters/0-2-introduction|0.2. Introduction]] and [[wiki/chapters/1-getting-started|1. Getting Started]].
- Updated `source_path` metadata and visible Path lines to the numbered raw filenames.
- Fixed internal wikilinks in index, overview, chapter pages, examples, tool notes, and log entries.

## 2026-05-06 ingest | Rust Book Chapter 2

- Ingested `raw/books/the_rust_programming_language/2. Programming a Guessing Game.md`.
- Created source summary [[wiki/sources/2-programming-a-guessing-game]].
- Created chapter page [[wiki/chapters/2-programming-a-guessing-game|2. Programming a Guessing Game]] and project page [[guessing-game]].
- Created crate page [[rand]] and compiler error page [[e0308-mismatched-types|E0308 mismatched types]].
- Created concept pages [[variables-and-mutability|variables and mutability]], [[shadowing]], [[result|Result]], [[match]], [[loop]], and [[crate]].
- Updated [[cargo|Cargo]], `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 2 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 3

- Ingested `raw/books/the_rust_programming_language/3. Common Programming Concepts.md`.
- Ingested `raw/books/the_rust_programming_language/3.1. Variables and Mutability.md`.
- Ingested `raw/books/the_rust_programming_language/3.2. Data Types.md`.
- Ingested `raw/books/the_rust_programming_language/3.3. Functions.md`.
- Ingested `raw/books/the_rust_programming_language/3.4. Comments.md`.
- Ingested `raw/books/the_rust_programming_language/3.5. Control Flow.md`.
- Created source summaries [[wiki/sources/3-common-programming-concepts]], [[3-1-variables-and-mutability]], [[3-2-data-types]], [[3-3-functions]], [[3-4-comments]], and [[3-5-control-flow]].
- Created chapter page [[wiki/chapters/3-common-programming-concepts|3. Common Programming Concepts]] and exercise page [[chapter-3-practice]].
- Created concept pages [[data-types|data types]], [[scalar-types|scalar types]], [[compound-types|compound types]], [[functions]], [[statements-and-expressions|statements and expressions]], [[comments]], [[control-flow|control flow]], [[if-expressions|if expressions]], and [[constants]].
- Updated concept pages [[variables-and-mutability|variables and mutability]], [[shadowing]], and [[loop]].
- Created error pages [[e0284-type-annotations-needed|E0284 type annotations needed]] and [[e0384-cannot-assign-twice|E0384 cannot assign twice]], and expanded [[e0308-mismatched-types|E0308 mismatched types]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 3 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 4 opening and ownership

- Ingested `raw/books/the_rust_programming_language/4. Understanding Ownership.md`.
- Ingested `raw/books/the_rust_programming_language/4.1. What is Ownership?.md`.
- Created source summaries [[wiki/sources/4-understanding-ownership]] and [[4-1-what-is-ownership]].
- Created chapter page [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]].
- Created concept pages [[ownership]], [[stack-and-heap|stack and heap]], [[scope]], [[string-type|String]], [[drop]], [[move-semantics|move semantics]], [[clone]], and [[copy-trait|Copy trait]].
- Created comparison pages [[string-literal-vs-string]] and [[move-vs-clone]].
- Created compiler error page [[e0382-borrow-of-moved-value|E0382 borrow of moved value]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 4 ownership relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 4 references, borrowing, and slices

- Ingested `raw/books/the_rust_programming_language/4.2. References and Borrowing.md`.
- Ingested `raw/books/the_rust_programming_language/4.3. The Slice Type.md`.
- Created source summaries [[4-2-references-and-borrowing]] and [[4-3-the-slice-type]].
- Updated chapter page [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]] and concept page [[ownership]].
- Created concept pages [[reference]], [[borrowing]], [[mutable-reference|mutable reference]], [[dangling-reference|dangling reference]], [[lifetimes]], [[data-race|data race]], [[slices]], [[string-slice|String slice]], and [[deref-coercions|deref coercions]].
- Created pattern page [[api-design|API design]].
- Created compiler error pages [[e0596-cannot-borrow-as-mutable|E0596 cannot borrow as mutable]], [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]], [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]], and [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 4 borrowing and slice relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 maintenance | Wiki lint

- Ran lint checks for frontmatter, wikilinks, orphan pages, raw/source summary coverage, index coverage, duplicate stems, and code example density.
- Added frontmatter to [[log|Wiki Log]].
- Created `needs-review` concept, tool, and glossary stubs for missing wikilinks.
- Updated [[index|Rust Wiki Index]] so all wiki pages are cataloged.
- Appended new lint-generated relations to `infranodus/rust-book-relations.txt`.
- Saved lint report to `output/2026-05-06-wiki-lint-report.md`.

## 2026-05-06 ingest | Rust Book Chapter 5 opening and structs

- Ingested `raw/books/the_rust_programming_language/5. Using Structs to Structure Related Data.md`.
- Ingested `raw/books/the_rust_programming_language/5.1. Defining and Instantiating Structs.md`.
- Created source summaries [[wiki/sources/5-using-structs-to-structure-related-data]] and [[5-1-defining-and-instantiating-structs]].
- Created chapter page [[wiki/chapters/5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]].
- Created concept pages [[structs]], [[struct-fields|struct fields]], [[struct-instances|struct instances]], [[field-init-shorthand]], [[struct-update-syntax]], [[tuple-structs]], [[unit-like-structs]], [[associated-functions]], [[methods]], [[domain-modeling|domain modeling]], and [[type-checking|type checking]].
- Updated [[ownership]], [[move-semantics|move semantics]], [[copy-trait|Copy trait]], [[tuple]], [[unit-type|unit type]], [[string-type|String]], [[string-slice|String slice]], [[lifetimes]], and [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 5 struct relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 5 rectangle example and methods

- Ingested `raw/books/the_rust_programming_language/5.2. An Example Program Using Structs.md`.
- Ingested `raw/books/the_rust_programming_language/5.3. Methods.md`.
- Created source summaries [[5-2-an-example-program-using-structs]] and [[5-3-methods]].
- Expanded chapter page [[wiki/chapters/5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]].
- Created example page [[rectangle-example|rectangle area example]].
- Created concept pages [[debug-trait|Debug trait]], [[derive-attribute|derive attribute]], [[display-formatting|Display formatting]], [[debug-formatting|Debug formatting]], [[dbg-macro|dbg! macro]], [[impl-block|impl block]], [[method-syntax|method syntax]], [[self-parameter|self parameter]], [[self-type|Self type]], [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]], [[multiple-impl-blocks|multiple impl blocks]], [[constructor-functions|constructor functions]], and [[getters]].
- Created glossary pages [[stdout]], [[stderr]], and [[enum]].
- Created compiler error page [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]].
- Updated [[methods]], [[associated-functions]], [[structs]], [[traits]], [[borrowing]], [[reference]], and [[println-macro|println! macro]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 5 method/debug relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 question | Wiki usage workflow

- Answered how the wiki should be used by the user and by the LLM.
- Saved the answer as [[wikidan-qanday-foydalanish|Wikidan qanday foydalanish mumkin?]].
- Updated `wiki/index.md`.

## 2026-05-06 question | Rust variables

- Answered the user's Uzbek question about Rust variables.
- Saved the answer as [[ozgaruvchilar-haqida|Rustda o'zgaruvchilar haqida]].
- Updated `wiki/index.md`.
- Appended question relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 question | Rust variables details

- Expanded [[ozgaruvchilar-haqida|Rustda o'zgaruvchilar haqida]] with clearer coverage of binding, `mut`, `const`, and `shadowing`.
- Added a short rule-of-thumb section and linked the explanation more explicitly to [[ownership]], [[borrowing]], and [[lifetimes]].
- Appended two small relations to `infranodus/rust-book-relations.txt` for [[variables-and-mutability|variables and mutability]] and [[immutability]].

## 2026-05-06 question | Ownership

- Saved a new answer page [[ownership-haqida|Ownership nima?]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 question | Stack and heap

- Saved a new answer page [[stack-va-heap-nima|Stack va heap nima?]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 question | Borrowing

- Saved a new answer page [[borrowing-nima-va-qanday-ishlaydi|Borrowing nima va qanday ishlaydi?]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 question | Lifetimes

- Saved a new answer page [[lifetimes-nima-va-nima-uchun-kerak|Lifetimes nima va nima uchun kerak?]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 question | Generic lifetime

- Saved a new answer page [[generic-lifetime-qanday-ishlatiladi|Generic lifetime qanday ishlatiladi?]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 ingest | Rust Book Chapter 6 opening and defining an enum

- Ingested `raw/books/the_rust_programming_language/6. Enums and Pattern Matching.md`.
- Ingested `raw/books/the_rust_programming_language/6.1. Defining an Enum.md`.
- Created source summaries [[wiki/sources/6-enums-and-pattern-matching]] and [[6-1-defining-an-enum]].
- Created chapter page [[wiki/chapters/6-enums-and-pattern-matching|6. Enums and Pattern Matching]].
- Created concept pages [[enums]], [[enum-variants|enum variants]], [[option|Option]], and [[null-billion-dollar-mistake|null va billion dollar mistake]].
- Created example pages [[ip-addr-enum|IP address enum example]] and [[message-enum|Message enum example]].
- Created glossary pages [[prelude]] and [[variant]].
- Updated [[methods]] (enum'ga method qo'shish), [[generics]] (`Option<T>` example bilan), [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]] (Option<i8> + i8 case), and glossary [[enum]] (concept page'ga redirect).
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 6 enum relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 6 match, if let, and let...else

- Ingested `raw/books/the_rust_programming_language/6.2. The match Control Flow Construct.md`.
- Ingested `raw/books/the_rust_programming_language/6.3. Concise Control Flow with if let and let...else.md`.
- Created source summaries [[6-2-the-match-control-flow-construct]] and [[6-3-concise-control-flow-with-if-let-and-let-else]].
- Expanded chapter page [[wiki/chapters/6-enums-and-pattern-matching|6. Enums and Pattern Matching]].
- Created concept pages [[if-let|if let]], [[let-else|let...else]], [[exhaustive-matching|exhaustive matching]], and [[catch-all-patterns|catch-all patterns]].
- Created compiler error page [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]].
- Created example page [[coin-match|Coin match example]].
- Updated [[match]], [[pattern-matching|pattern matching]], [[option|Option]], [[enums]], [[enum-variants|enum variants]], and [[control-flow|control flow]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 6 match/if-let relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 7 opening and packages/crates

- Ingested `raw/books/the_rust_programming_language/7. Packages, Crates, and Modules.md`.
- Ingested `raw/books/the_rust_programming_language/7.1. Packages and Crates.md`.
- Created source summaries [[wiki/sources/7-packages-crates-and-modules]] and [[7-1-packages-and-crates]].
- Created chapter page [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]].
- Created concept pages [[package]], [[binary-crate|binary crate]], [[library-crate|library crate]], [[crate-root|crate root]], [[module-system|module system]], [[paths]], and [[use-declarations|use declarations]].
- Updated [[crate]], [[module]], [[scope]], [[dependencies]], [[cargo|Cargo]], [[cargo-toml|Cargo.toml]], and [[rust-learning-roadmap]].
- Updated `wiki/index.md` and `wiki/overview.md`.
- Appended Chapter 7 package/crate relations to `infranodus/rust-book-relations.txt`.

## 2026-05-06 ingest | Rust Book Chapter 7.2 modules, scope, and privacy

- Ingested `raw/books/the_rust_programming_language/7.2. Control Scope and Privacy with Modules.md`.
- Created source summary [[7-2-control-scope-and-privacy-with-modules]].
- Expanded chapter page [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]].
- Created concept pages [[module-tree|module tree]], [[privacy]], and [[pub-keyword|pub keyword]].
- Created example page [[backyard-module-layout|backyard module layout]].
- Updated [[module]], [[module-system|module system]], [[scope]], [[paths]], [[use-declarations|use declarations]], [[crate-root|crate root]], and [[rust-learning-roadmap]].
- Updated `wiki/index.md` and `wiki/overview.md`.
- Appended Chapter 7.2 module/privacy relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-06-chapter-7-2-ingest-report.md`.

## 2026-05-06 ingest | Rust Book Chapter 7.3 paths and public API

- Ingested `raw/books/the_rust_programming_language/7.3. Paths for Referring to an Item in the Module Tree.md`.
- Created source summary [[7-3-paths-for-referring-to-an-item-in-the-module-tree]].
- Expanded chapter page [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]].
- Created concept pages [[absolute-path|absolute path]], [[relative-path|relative path]], [[super-keyword|super keyword]], and [[public-api|public API]].
- Created compiler error page [[e0603-private-item|E0603 private item]].
- Created example page [[restaurant-paths-and-privacy|restaurant paths and privacy]].
- Updated [[paths]], [[privacy]], [[pub-keyword|pub keyword]], [[module-tree|module tree]], [[module-system|module system]], [[structs]], [[struct-fields|struct fields]], [[enums]], [[enum-variants|enum variants]], [[binary-crate|binary crate]], [[library-crate|library crate]], [[api-design|API design]], and [[rust-learning-roadmap]].
- Updated `wiki/index.md` and `wiki/overview.md`.
- Appended Chapter 7.3 path/privacy/API relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-06-chapter-7-3-ingest-report.md`.

## 2026-05-06 question | Control flow basics

- Saved a new answer page [[if-loop-while-for-va-ularga-bogliq-control-flow|if, loop, while, for va ularga bog'liq control flow]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 question | Control flow practice examples

- Expanded [[if-loop-while-for-va-ularga-bogliq-control-flow|if, loop, while, for va ularga bog'liq control flow]] with practical examples for `if` vs `match`, `loop` + `break` + `continue`, `while` vs `for`, and `for` ownership.

## 2026-05-06 question | Traits

- Expanded [[traits]] with explicit `impl`, trait bound, and `where` examples.
- Saved a new answer page [[traits-haqida|Traitlar haqida]].
- Updated `wiki/index.md` to surface the new question under Questions.

## 2026-05-06 ingest | Rust Book Chapter 7.4 use and 7.5 module files

- Ingested `raw/books/the_rust_programming_language/7.4. Bringing Paths Into Scope with the use Keyword.md`.
- Ingested `raw/books/the_rust_programming_language/7.5. Separating Modules into Different Files.md`.
- Created source summaries [[7-4-bringing-paths-into-scope-with-the-use-keyword]] and [[7-5-separating-modules-into-different-files]].
- Expanded chapter page [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]].
- Created concept pages [[pub-use|pub use]], [[use-aliases|use aliases]], [[nested-use-paths|nested use paths]], [[glob-operator|glob operator]], [[module-file-layout|module file layout]], and [[mod-declarations|mod declarations]].
- Created example page [[restaurant-reexports-and-file-layout|restaurant re-exports and file layout]].
- Updated [[use-declarations|use declarations]], [[module-system|module system]], [[module]], [[module-tree|module tree]], [[paths]], [[scope]], [[privacy]], [[pub-keyword|pub keyword]], [[public-api|public API]], [[crate]], [[dependencies]], [[standard-library|standard library]], [[rand]], [[cargo-toml|Cargo.toml]], [[api-design|API design]], and [[rust-learning-roadmap]].
- Updated `wiki/index.md` and `wiki/overview.md`.
- Appended Chapter 7.4-7.5 relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-06-chapter-7-4-7-5-ingest-report.md`.

## 2026-05-06 ingest | Rust Book Chapter 8 opening and vectors

- Ingested `raw/books/the_rust_programming_language/8. Common Collections.md`.
- Ingested `raw/books/the_rust_programming_language/8.1 Storing Lists of Values with Vectors.md`.
- Created source summaries [[wiki/sources/8-common-collections]] and [[8-1-storing-lists-of-values-with-vectors]].
- Created chapter page [[wiki/chapters/8-common-collections|8. Common Collections]].
- Created concept pages [[collections]], [[vector|Vec<T>]], [[vec-macro|vec! macro]], [[vector-indexing|vector indexing]], [[hash-map|HashMap]], [[panic]], and [[dereference-operator|dereference operator]].
- Created example pages [[vector-basics|vector basics]] and [[spreadsheet-cell-vector|spreadsheet cell vector]].
- Updated [[array]], [[generics]], [[type-inference|type inference]], [[type-annotations|type annotations]], [[drop]], [[standard-library|standard library]], [[string-type|String]], [[stack-and-heap|stack and heap]], [[borrowing]], [[mutable-reference|mutable reference]], [[enums]], [[option|Option]], [[for-loop|for loop]], and [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 8 and 8.1 relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-06-chapter-8-8-1-ingest-report.md`.

## 2026-05-06 ingest | Rust Book Chapter 8.2 strings

- Ingested `raw/books/the_rust_programming_language/8.2. Storing UTF-8 Encoded Text with Strings.md`.
- Created source summary [[8-2-storing-utf-8-encoded-text-with-strings]].
- Expanded chapter page [[wiki/chapters/8-common-collections|8. Common Collections]].
- Created concept pages [[utf-8|UTF-8]], [[string-concatenation|string concatenation]], [[format-macro|format! macro]], [[string-indexing|string indexing]], and [[grapheme-clusters|grapheme clusters]].
- Created tool page [[crates-io|crates.io]].
- Created example pages [[string-updating-and-concatenation|string updating and concatenation]] and [[utf8-string-iteration|UTF-8 string iteration]].
- Updated [[string-type|String]], [[string-slice|String slice]], [[collections]], [[deref-coercions|deref coercions]], [[move-semantics|move semantics]], [[display-formatting|Display formatting]], [[slices]], [[char-type|char type]], [[string-literal-vs-string]], [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]], and [[rust-learning-roadmap]].
- Updated `wiki/index.md` and `wiki/overview.md`.
- Appended Chapter 8.2 relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-06-chapter-8-2-ingest-report.md`.

## 2026-05-06 ingest | Rust Book Chapter 8.3 hash maps

- Ingested `raw/books/the_rust_programming_language/8.3. Storing Keys with Associated Values in Hash Maps.md`.
- Created source summary [[8-3-storing-keys-with-associated-values-in-hash-maps]].
- Expanded chapter page [[wiki/chapters/8-common-collections|8. Common Collections]] and core [[hash-map|HashMap]] concept page.
- Created concept pages [[entry-api|entry API]] and [[hashing-function|hashing function]].
- Created example pages [[team-scores-hashmap|team scores hashmap]] and [[word-count-hashmap|word count hashmap]].
- Created exercise page [[chapter-8-collections-practice|Chapter 8 collections practice]].
- Updated [[collections]], [[ownership]], [[copy-trait|Copy trait]], [[borrowing]], [[dereference-operator|dereference operator]], [[option|Option]], [[use-declarations|use declarations]], [[standard-library|standard library]], and [[e0382-borrow-of-moved-value|E0382 borrow of moved value]].
- Updated `wiki/index.md`, `wiki/overview.md`, and [[rust-learning-roadmap]].
- Appended Chapter 8.3 relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-06-chapter-8-3-ingest-report.md`.

## 2026-05-06 maintenance | Wiki health lint

- Ran lint checks for frontmatter, allowed `type`/`status`, wikilinks, orphan pages, index coverage, raw/source matching, duplicate stems, filename convention, `source_count`, trailing whitespace, review backlog, and snippet candidates.
- Fixed broken links in [[grapheme-clusters|grapheme clusters]] and [[log|Wiki Log]].
- Updated stale Chapter 8 overview heading after 8.3 completion.
- Confirmed 276 wiki pages, 36 source summaries, 36 raw book markdown files, 0 missing wikilinks, 0 orphan pages, and 0 index coverage gaps.
- Updated lint report at `output/2026-05-06-wiki-lint-report.md`.

## 2026-05-07 maintenance | Wiki lint

- `frontmatter`, allowed `type` va `status`, wikilinks, orphan pages, raw/source summary coverage, index coverage, duplicate stems, filename style, trailing whitespace va zich Rust code examples bo'yicha lint qilindi.
- [[overview|Rust Wiki Overview]] va [[log|Wiki Log]] core pages sifatida [[index|Rust Wiki Index]]ga qo'shildi.
- `Clippings/` ichida 32 ta hali ingest qilinmagan Rust Book clipping file topildi; ingest scope qarori kerak bo'lgani uchun joyida qoldirildi.
- Hisobot `output/2026-05-07-wiki-lint-report.md`ga saqlandi.

## 2026-05-07 ingest | Rust Book Chapter 9 opening and panic

- Ingested `raw/books/the_rust_programming_language/9. Error Handling.md`.
- Ingested `raw/books/the_rust_programming_language/9.1. Unrecoverable Errors with panic!.md`.
- Created source summaries [[wiki/sources/9-error-handling]] and [[9-1-unrecoverable-errors-with-panic]].
- Created chapter page [[wiki/chapters/9-error-handling|9. Error Handling]].
- Created concept pages [[recoverable-errors|recoverable errors]], [[unrecoverable-errors|unrecoverable errors]], [[backtrace]], [[stack-unwinding|stack unwinding]], [[abort-on-panic|abort on panic]], and [[buffer-overread|buffer overread]].
- Updated [[error-handling]], [[panic|panic!]], [[result|Result<T, E>]], [[vector-indexing|vector indexing]], [[cargo-toml|Cargo.toml]], [[release-build|release build]], [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]], and [[rust-learning-roadmap]].
- Appended Chapter 9.1 error-handling and panic relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-07-chapter-9-9-1-ingest-report.md`.

## 2026-05-07 ingest | Rust Book Chapter 9.2 Result

- Ingested `raw/books/the_rust_programming_language/9.2. Recoverable Errors with Result.md`.
- Created source summary [[9-2-recoverable-errors-with-result]].
- Expanded chapter page [[wiki/chapters/9-error-handling|9. Error Handling]] with `Result<T, E>`, `File::open`, `ErrorKind`, `unwrap`, `expect`, error propagation, `?`, and `main -> Result<(), E>`.
- Created concept pages [[error-propagation|error propagation]], [[question-mark-operator|question mark operator]], [[unwrap]], [[expect]], [[error-kind|ErrorKind]], [[io-error|io::Error]], [[file-handle|file handle]], [[from-trait|From trait]], and [[box-dyn-error|Box<dyn Error>]].
- Updated [[result|Result<T, E>]], [[recoverable-errors|recoverable errors]], [[error-handling]], [[panic|panic!]], [[option|Option]], [[match]], [[main-function|main function]], [[generics]], [[traits]], and [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]], and [[rust-learning-roadmap]].
- Appended Chapter 9.2 error-handling relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-07-chapter-9-2-ingest-report.md`.

## 2026-05-07 ingest | Rust Book Chapter 9.3 panic or Result

- Ingested `raw/books/the_rust_programming_language/9.3. To panic! or Not to panic!.md`.
- Created source summary [[9-3-to-panic-or-not-to-panic]].
- Completed chapter page [[wiki/chapters/9-error-handling|9. Error Handling]] with panic-vs-Result decision criteria, examples/prototypes/tests guidance, bad state, contracts, invariants, and custom validation types.
- Created concept pages [[panic-vs-result|panic! vs Result]], [[bad-state|bad state]], [[invariants]], and [[function-contracts|function contracts]].
- Created pattern page [[custom-validation-types|custom validation types]] and example page [[guess-validation-type|Guess validation type]].
- Updated [[panic|panic!]], [[result|Result<T, E>]], [[recoverable-errors|recoverable errors]], [[unrecoverable-errors|unrecoverable errors]], [[expect]], [[unwrap]], [[option|Option]], [[type-checking|type checking]], [[api-design|API design]], and [[guessing-game|Guessing Game]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]], and [[rust-learning-roadmap]].
- Appended Chapter 9.3 decision and validation relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-07-chapter-9-3-ingest-report.md`.

## 2026-05-07 ingest | Rust Book Chapter 10 opening

- Ingested `raw/books/the_rust_programming_language/10. Generic Types, Traits, and Lifetimes.md`.
- Created source summary [[wiki/sources/10-generic-types-traits-and-lifetimes]].
- Created chapter page [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]].
- Created concept pages [[code-duplication|code duplication]], [[function-extraction|function extraction]], [[generic-type-parameters|generic type parameters]], and [[abstraction]].
- Created example page [[largest-function-extraction|largest function extraction]].
- Updated [[generics]], [[traits]], [[lifetimes]], [[functions]], [[slices]], and [[zero-cost-abstractions|zero-cost abstractions]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]], and [[rust-learning-roadmap]].
- Appended Chapter 10 opening relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-07-chapter-10-ingest-report.md`.

## 2026-05-07 ingest | Rust Book Chapter 10.1 generic data types

- Ingested `raw/books/the_rust_programming_language/10.1. Generic Data Types.md`.
- Created source summary [[10-1-generic-data-types]].
- Expanded chapter page [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]] with generic function, struct, enum, method, and monomorphization details.
- Created concept pages [[generic-functions|generic functions]], [[generic-structs|generic structs]], [[generic-enums|generic enums]], [[generic-methods|generic methods]], [[monomorphization]], and [[partial-ord|PartialOrd]].
- Created example pages [[generic-largest-function|generic largest function]] and [[generic-point|generic Point]].
- Created error page [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]] and updated [[e0308-mismatched-types|E0308 mismatched types]] with `Point<T>` context.
- Updated [[generics]], [[generic-type-parameters|generic type parameters]], [[traits]], [[functions]], [[structs]], [[methods]], [[enums]], [[option|Option<T>]], [[result|Result<T, E>]], and [[zero-cost-abstractions|zero-cost abstractions]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]], and [[rust-learning-roadmap]].
- Appended Chapter 10.1 generic data type relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-07-chapter-10-1-ingest-report.md`.

## 2026-05-07 ingest | Rust Book Chapter 10.2 traits

- Ingested `raw/books/the_rust_programming_language/10.2. Defining Shared Behavior with Traits.md`.
- Created source summary [[10-2-defining-shared-behavior-with-traits]].
- Expanded chapter page [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]] with trait definitions, implementations, default implementations, trait bounds, `impl Trait`, `where` clauses, orphan rule, conditional methods, and blanket implementations.
- Created concept pages [[trait-definitions|trait definitions]], [[trait-implementations|trait implementations]], [[default-trait-implementations|default trait implementations]], [[trait-bounds|trait bounds]], [[impl-trait|impl Trait]], [[where-clauses|where clauses]], [[orphan-rule|orphan rule]], [[blanket-implementations|blanket implementations]], and [[conditional-method-implementations|conditional method implementations]].
- Created example pages [[summary-trait-example|Summary trait example]], [[notify-trait-parameters|notify trait parameters]], and [[pair-conditional-methods|Pair conditional methods]].
- Updated [[traits]], [[generics]], [[generic-functions|generic functions]], [[generic-type-parameters|generic type parameters]], [[generic-methods|generic methods]], [[partial-ord|PartialOrd]], [[impl-block|impl block]], [[public-api|public API]], [[display-formatting|Display formatting]], [[zero-cost-abstractions|zero-cost abstractions]], and [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]], and [[rust-learning-roadmap]].
- Appended Chapter 10.2 trait relations to `infranodus/rust-book-relations.txt`.
- Saved ingest report to `output/2026-05-07-chapter-10-2-ingest-report.md`.

## 2026-05-07 ingest | 10.3. Validating References with Lifetimes

- Ingested `raw/books/the_rust_programming_language/10.3. Validating References with Lifetimes.md`.
- Created [[10-3-validating-references-with-lifetimes]] in `wiki/sources/`.
- Created new concept pages: [[lifetime-elision]], [[static-lifetime]].
- Updated [[lifetimes]] — to'liq kengaytirildi: borrow checker, funksiya/struct/method annotatsiyalar, elision, `'static`, status `stable`ga o'zgartirildi.
- Updated [[e0106-missing-lifetime-specifier]] — Chapter 10.3 holati (ikkita reference parametr va return) qo'shildi.
- Created new error pages: [[e0597-does-not-live-long-enough]], [[e0515-cannot-return-local-reference]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]].
- Appended Chapter 10.3 lifetime relations to `infranodus/rust-book-relations.txt`.
- Chapter 10 to'liq yakunlandi.

## 2026-05-07 ingest | 11. Writing Automated Tests + 11.1. How to Write Tests

- Ingested `raw/books/the_rust_programming_language/11. Writing Automated Tests.md`.
- Ingested `raw/books/the_rust_programming_language/11.1. How to Write Tests.md`.
- Created [[wiki/sources/11-writing-automated-tests]] va [[11-1-how-to-write-tests]] in `wiki/sources/`.
- Created new concept pages: [[testing]], [[test-macros]], [[should-panic]], [[cfg-test]].
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]] — Chapter 11 synthesis boshlandi.
- Appended Chapter 11 test relations to `infranodus/rust-book-relations.txt`.

## 2026-05-07 ingest | 11.2. Controlling How Tests Are Run

- Ingested `raw/books/the_rust_programming_language/11.2. Controlling How Tests Are Run.md`.
- Created [[11-2-controlling-how-tests-are-run]] in `wiki/sources/`.
- Created new concept page: [[test-filtering]].
- Updated [[testing]] — parallel/sequential, output capture, ignore, filtrlash bo'limlari qo'shildi.
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]] — Chapter 11.2 synthesis qo'shildi.
- Appended Chapter 11.2 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-07 ingest | 11.3. Test Organization

- Ingested `raw/books/the_rust_programming_language/11.3. Test Organization.md`.
- Created [[11-3-test-organization]] in `wiki/sources/`.
- Created new concept pages: [[unit-tests]], [[integration-tests]].
- Updated [[testing]] — unit/integration jadval va havola qo'shildi.
- Updated [[index|Rust Wiki Index]], [[overview|Rust Wiki Overview]] — Chapter 11 to'liq yakunlandi.
- Appended Chapter 11.3 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-07 lint | Wiki sog'liq tekshiruvi

- Tekshirildi: 340 sahifa, frontmatter, singan linklar, orphan sahifalar, raw/sources qoplamasi.
- Yaratildi: [[borrow-checker]] konsept sahifasi (`wiki/concepts/borrow-checker.md`).
- Yaratildi: [[correctness]] konsept sahifasi (`wiki/concepts/correctness.md`).
- Tuzatildi: 5 ta source sahifadagi `wiki/index.md` -> [[index]], `wiki/overview.md` -> [[overview]] (path-style linklar slug-style ga o'zgartirildi).

## 2026-05-07 ingest | Rust Book Chapter 12 — 12, 12.1, 12.2

- Ingested `raw/books/the_rust_programming_language/12. An IO Project Building a Command Line Program.md`.
- Ingested `raw/books/the_rust_programming_language/12.1. Accepting Command Line Arguments.md`.
- Ingested `raw/books/the_rust_programming_language/12.2. Reading a File.md`.
- Created source summaries [[12-an-io-project-building-a-command-line-program]], [[12-1-accepting-command-line-arguments]], and [[12-2-reading-a-file]] in `wiki/sources/`.
- Created chapter page [[12-an-io-project|12. An I/O Project: Building a Command Line Program]].
- Created concept pages [[env-args|std::env::args]], [[fs-read-to-string|fs::read_to_string]], and [[separation-of-concerns|separation of concerns]].
- Updated [[index|Rust Wiki Index]] with new sources, chapter, and concepts.
- Appended Chapter 12 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-07 ingest | Rust Book Chapter 12.3 — Refactoring

- Ingested `raw/books/the_rust_programming_language/12.3. Refactoring to Improve Modularity and Error Handling.md`.
- Created source summary [[12-3-refactoring-to-improve-modularity-and-error-handling]] in `wiki/sources/`.
- Created concept pages [[unwrap-or-else|unwrap_or_else]] va [[process-exit|process::exit]].
- Updated [[separation-of-concerns]] — to'liq refactoring recipe qo'shildi (binary project uchun qoida).
- Updated [[constructor-functions]] — `new` vs `build` naming convention qo'shildi.
- Updated [[12-an-io-project]] chapter page — 12.3 bo'limi va yangi review questions qo'shildi.
- Updated [[index|Rust Wiki Index]] — yangi source va 2 ta concept qo'shildi; `source_count: 52`.
- Appended Chapter 12.3 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-07 ingest | Rust Book Chapter 12.4 — TDD va search

- Ingested `raw/books/the_rust_programming_language/12.4. Adding Functionality with Test Driven Development.md`.
- Created source summary [[12-4-adding-functionality-with-test-driven-development]] in `wiki/sources/`.
- Created concept page [[tdd|Test-Driven Development (TDD)]].
- Updated [[lifetimes]] — "bir parametrni return bilan bog'lash" misoli (`search` funksiyasi) qo'shildi; `source_count: 5`.
- Updated [[12-an-io-project]] chapter page — 12.4 bo'limi va TDD concept qo'shildi.
- Updated [[index|Rust Wiki Index]] — yangi source va TDD concept; `source_count: 53`.
- Appended Chapter 12.4 relations to `infranodus/rust-book-relations.txt`.

## 2026-05-07 lint | To'liq wiki sog'liq tekshiruvi #2

- Tekshirildi: 375 sahifa, frontmatter, singan linklar, orphan sahifalar, raw/sources qoplamasi.
- **Singan linklar:** 20 ta broken wikilink aniqlandi va 0 ga tushirildi.
  - Yaratildi: [[collect]], [[cli|CLI]], [[file-io|File I/O]], [[refactoring]] konsept sahifalari.
  - Yaratildi: [[minigrep-args]] va [[minigrep-read-file]] example sahifalari.
  - Tuzatildi: `single-responsibility` → [[separation-of-concerns|separation-of-concerns]] (`12-2` source).
  - Tuzatildi: `config-struct` → [[structs|structs]], `build-constructor` → [[constructor-functions|constructor-functions]] (`12-3` source).
  - Tuzatildi: `wiki/index.md` path-link → [[index]] (`12-5`, `12-6` sources).
  - Tuzatildi: `environment-variables` → [[env-var]] (`12-an-io-project` source).
  - Tuzatildi: `11-writing-automated-tests` → [[wiki/sources/11-writing-automated-tests]] (`12-an-io-project` chapter).
- **Orphan sahifalar:** 0.
- **Frontmatter:** hamma sahifalarda to'liq.
- Updated [[index|Rust Wiki Index]] — 4 yangi konsept, 2 yangi example qo'shildi.

## 2026-05-07 optimize | Wiki bo'shliqlari va muammolar bartaraf etildi

- **Status optimallashtirish:** 73 sahifa `needs-review`/`draft` → `active` (barcha 35 glossary, 22 concept, 5 tool, 11 draft).
  - Qoldi `needs-review`: [[async-await]] va [[concurrency]] — kontent keyingi boblarda to'ldiriladi (ch16).
- **Yaratildi:** [[wiki/chapters/11-writing-automated-tests|11. Writing Automated Tests]] bob sahifasi (11.1–11.3 source summarylaridan).
- Updated [[index|Rust Wiki Index]] — chapter 11 qo'shildi.

**Yakuniy holat:** 385 sahifa, 0 singan link, 0 orphan, 2 ta `needs-review` (intentional), 0 `draft`.

## 2026-05-08 query | Rust variable qo'llanmasi

- Updated [[ozgaruvchilar-haqida]] — `let`, `mut`, [[shadowing]], [[constants]], [[type-inference]], [[type-annotations]], [[scope]], destructuring, `_` ignore pattern, va [[ownership]] bilan bog'liqligi kengaytirildi.
- Qo'shildi: amaliy tanlov qoidasi va ko'p uchraydigan compiler xatolari bo'limi ([[e0384-cannot-assign-twice|E0384]], [[e0308-mismatched-types|E0308]], [[e0596-cannot-borrow-as-mutable|E0596]]).

## 2026-05-08 query | const va static farqi

- Created concept page [[static-items]] — `static` keyword uchun alohida mental model, single allocation, `static mut`, va `const`dan farqlari yozildi.
- Created question page [[const-va-static-farqi]] — `const` vs `static`, address identity, lifetime, mutability, va qachon qaysi biri tanlanishi tushuntirildi.
- Updated [[index|Rust Wiki Index]] — yangi concept va question qo'shildi.

## 2026-05-08 ingest | 16. Fearless Concurrency + 16.1. Using Threads

- Ingested `raw/books/the_rust_programming_language/16. Fearless Concurrency.md`.
- Ingested `raw/books/the_rust_programming_language/16.1. Using Threads to Run Code Simultaneously.md`.
- Created [[wiki/sources/16-fearless-concurrency]] — fearless concurrency falsafasi, 4 mavzu overview, Erlang vs Rust taqqoslamasi.
- Created [[16-1-using-threads-to-run-code-simultaneously]] — thread::spawn, JoinHandle, join joyi, move closures, E0373/E0382.
- Created [[wiki/chapters/16-fearless-concurrency]] — bob sahifasi, 16.0 + 16.1 synthesis, 5 mashq, 6 review savol.
- Created concept pages:
  - [[threads]] — aktiv; 1:1 model, thread::spawn, lifecycle.
  - [[join-handle]] — aktiv; JoinHandle<T>, .join() semantikasi, join joyi.
  - [[move-closures-threads|move closures (threads)]] — aktiv; move, E0373, E0382 farqi.
  - [[send-trait|Send trait]] — needs-review stub (ch16.4 da to'ldiriladi).
  - [[sync-trait|Sync trait]] — needs-review stub (ch16.4 da to'ldiriladi).
- Created error page [[e0373-closure-may-outlive-current-function|E0373]] — closure borrows across thread boundary; fix: move.
- Created example page [[thread-spawn-basic]] — Listing 16-1/16-2/16-3 taqqoslama jadvali.
- Updated [[concurrency]]: needs-review → active; fearless concurrency tushunchasi, threadlar, Send/Sync; source_count 1→3.
- Updated [[closures]]: thread kontekstida move closure bo'limi qo'shildi; [[move-closures-threads]] linked.
- Updated [[rc-t|Rc<T>]]: Arc<T> thread-safe alternativ tavsifi kengaytirildi; Send trait eslatmasi.
- Updated [[interior-mutability]]: Mutex<T> thread-safe alternativ eslatmasi qo'shildi.
- Updated [[data-race]]: thread kontekstida data race bo'limi qo'shildi; source_count++.
- Updated [[e0382-borrow-of-moved-value|E0382]]: thread + move kontekstida E0382 stsenariysi qo'shildi.
- Updated [[index|Rust Wiki Index]]: 2 yangi source, 1 yangi chapter, 5 yangi concept, 1 yangi error; source_count 73→75.
- Updated `infranodus/rust-book-relations.txt`: 18 ta yangi relatsiya qo'shildi (ch16.0 + ch16.1).

**Yakuniy holat:** `needs-review`: [[send-trait]], [[sync-trait]], [[async-await]] (ch16.4/ch17 da to'ldiriladi).

## 2026-05-08 ingest | 16.2. Transfer Data Between Threads with Message Passing

- Ingested `raw/books/the_rust_programming_language/16.2. Transfer Data Between Threads with Message Passing.md`.
- Created [[16-2-transfer-data-between-threads-with-message-passing]] — channels, mpsc, send/recv, ownership transfer, multiple producers.
- Created concept pages:
  - [[channels]] — aktiv; mpsc::channel, tx/rx, send, recv, try_recv, iterator pattern, multiple producers via clone.
  - [[message-passing]] — aktiv; Go-style concurrency falsafasi, message-passing vs shared state.
- Created example page [[channel-basic-messages]] — Listing 16-7..16-11 to'plami.
- Updated [[wiki/chapters/16-fearless-concurrency]]: 16.2 bo'limi qo'shildi; mashqlar 5→8, review savollari 6→10; source_count 2→3.
- Updated [[threads]]: threadlar orasida muloqot pattern bo'limi qo'shildi (channels va shared state); source_count 2→3.
- Updated [[concurrency]]: channels va message-passing linklari qo'shildi; source_count 3→4.
- Updated [[e0382-borrow-of-moved-value|E0382]]: channel send kontekstida E0382 stsenariysi qo'shildi.
- Updated [[index|Rust Wiki Index]]: 1 yangi source, 2 yangi concept, 2 yangi example (thread-spawn-basic + channel-basic-messages); source_count 75→76.
- Updated `infranodus/rust-book-relations.txt`: 16.2 bo'limi va ~12 ta yangi relatsiya qo'shildi.

## 2026-05-08 ingest | 16.3. Shared-State Concurrency + 16.4. Extensible Concurrency with Send and Sync

- Ingested `raw/books/the_rust_programming_language/16.3. Shared-State Concurrency.md`.
- Ingested `raw/books/the_rust_programming_language/16.4. Extensible Concurrency with Send and Sync.md`.
- Created [[16-3-shared-state-concurrency]] — Mutex<T>, MutexGuard, Arc<T>, Rc xatosi, deadlock.
- Created [[16-4-extensible-concurrency-with-send-and-sync]] — Send, Sync to'liq ta'rif, manual impl unsafe.
- Created concept pages:
  - [[mutex-t|Mutex<T>]] — aktiv; lock/unlock, MutexGuard, interior mutability, deadlock, Arc<Mutex<T>>.
  - [[arc-t|Arc<T>]] — aktiv; atomic ref count, Send+Sync, Rc vs Arc taqqoslash.
- Updated [[send-trait|Send trait]]: needs-review → aktiv; to'liq kontent (Rc emas, Arc ha, auto-Send).
- Updated [[sync-trait|Sync trait]]: needs-review → aktiv; to'liq kontent (RefCell emas, Mutex ha).
- Created example page [[mutex-arc-counter]] — E0382 → E0277 → Arc<Mutex<T>> yechim progressi.
- Updated [[wiki/chapters/16-fearless-concurrency]]: 16.3 va 16.4 bo'limlari qo'shildi; source_count 3→5; mashqlar 10ta, review savollar 14ta.
- Updated [[interior-mutability]]: RefCell vs Mutex taqqoslash jadvali qo'shildi; source_count++.
- Updated [[concurrency]]: Mutex<T>, Arc<T> linklari; source_count 4→6.
- Updated [[e0277-trait-bound-not-satisfied|E0277]]: Rc<Mutex<T>> Send emas konteksti qo'shildi; source_count++.
- Updated [[index|Rust Wiki Index]]: 2 yangi source, 2 yangi concept, 1 yangi example; source_count 76→78.
- Updated `infranodus/rust-book-relations.txt`: 16.3 va 16.4 uchun yangi relatsiyalar qo'shildi.

**16. bob to'liq ingest yakunlandi.** `needs-review` qolmadi — [[send-trait]] va [[sync-trait]] aktiv.

## 2026-05-08 query | O'zgaruvchilar va ownership bog'lanishi

- Created [[ozgaruvchilar-let-mut-const-static-va-ownership]] — `let`, `let mut`, `const`, `static`, [[ownership]], [[borrowing]], [[move-semantics]], va [[copy-trait|Copy trait]] birgalikda maktab o'quvchisi darajasidagi mental model bilan tushuntirildi.
- Updated [[index|Rust Wiki Index]] — yangi question page qo'shildi.
- Updated `infranodus/rust-book-relations.txt` — variable/ownership/const/static bog'lanishlari uchun yangi relatsiyalar qo'shildi.
- Correction: Evidence bo'limidagi tashqi URLlar olib tashlandi; manbalar wiki ichidagi [[3-1-variables-and-mutability]], [[4-1-what-is-ownership]], [[4-2-references-and-borrowing]], va [[const-va-static-farqi]]ga tayandi.

## 2026-05-08 query | Ownership, borrowing, reference va lifetimes bog'lanishi

- Created [[ownership-borrowing-reference-va-lifetimes-boglanishi]] — ownership, borrowing, reference, va lifetimes bitta oqimda, maktabcha mental model va bir nechta Rust misollari bilan to'liq tushuntirildi.
- Updated [[index|Rust Wiki Index]] — yangi question page qo'shildi.
- Updated `infranodus/rust-book-relations.txt` — ownership/reference/lifetime munosabatlari uchun yangi relatsiyalar qo'shildi.
- Sources only from wiki and local raw book sections: [[4-1-what-is-ownership]], [[4-2-references-and-borrowing]], [[10-3-validating-references-with-lifetimes]], hamda `raw/books/the_rust_programming_language/4.1. What is Ownership?.md`, `4.2. References and Borrowing.md`, `10.3. Validating References with Lifetimes.md`.

## 2026-05-08 query | Qisqa savol-javob va amaliy mashq

- Created [[qisqa-savol-javob-variable-ownership-borrow-reference-lifetimes]] — variable, ownership, borrow, reference, va lifetimes bo'yicha juda qisqa savol-javob formati.
- Updated [[index|Rust Wiki Index]] — yangi short question page qo'shildi.
- Sources from wiki ichidagi concept/source sahifalarga tayandi.
- Added practice review to [[qisqa-savol-javob-variable-ownership-borrow-reference-lifetimes]] — foydalanuvchi yechimlari tekshirildi; `&str`, `&mut String`, `longest<'a>`, va `E0382` izohi bo'yicha qisqa feedback qo'shildi.

## 2026-05-08 lint | Wiki sog'lik tekshiruvi (Run 2 + Run 3 to'liq tozalash)

- Lint scope: 503 wiki sahifa, 94 raw fayl. CLAUDE.md lint workflow asosida.
- **Run 2 — minor link fixes (10 link):** `[[drop|...]]` retarget qilindi (6 fayl). `[[index]]` canonical wikilink'iga o'tildi (4 source fayl).
- **Run 3 — strukturaviy tozalash:**
  - Yangi: [[reference-counting]] concept page (3 ta buzilgan link uchun) va [[option-take]] snippet page.
  - Retarget: source 16-2 dagi `[[channels]]` izohi canonical target'ga o'tkazildi.
  - O'chirildi: `glossary/closures.md`, `glossary/iterators.md`, `glossary/testing.md` — concept dublikatlari, Obsidian'da basename noaniqligi sababi (105 noaniq hodisa hal bo'ldi).
  - Updated [[index|Rust Wiki Index]] — Glossary'dan 3 ta dublikat olib tashlandi, Concepts'ga reference-counting qo'shildi, Snippets bo'limi to'ldirildi.
- **Yakuniy holat:** buzilgan link 0, noaniq link 0, orphan 0, frontmatter 503/503 ✓, raw↔sources 94/94 ✓.
- Hisobot: `output/2026-05-08-wiki-lint-report.md`.

## 2026-05-08 ingest | 20. Advanced Features + 20.1. Unsafe Rust

- Ingested `raw/books/the_rust_programming_language/20. Advanced Features.md` va `raw/books/the_rust_programming_language/20.1. Unsafe Rust.md`.
- Created [[wiki/sources/20-advanced-features]] — Advanced features kirish bobi (5 mavzu sanab o'tiladi: unsafe, advanced traits, advanced types, advanced functions/closures, macros).
- Created [[wiki/sources/20-1-unsafe-rust]] — Unsafe Rust to'liq summary: 5 ta superpower, raw pointers, raw borrow operators, FFI (`extern "C"`, `safe fn`, `#[unsafe(no_mangle)]`), mutable static, unsafe trait, union, Miri, SAFETY konventsiyasi.
- Created [[wiki/chapters/20-advanced-features]] — kirish bob sahifasi.
- Created [[wiki/chapters/20-1-unsafe-rust]] — to'liq chapter: Learning Goal, 5 superpower jadvali, syntax misollar, safe abstraction (`split_at_mut`), FFI asoslari, mutable static, unsafe trait, Miri, 6 mashq, 10 review savollari.
- Created concept pages (10 ta):
  - [[raw-pointer]] — `*const T`/`*mut T`, reference bilan taqqoslash, common mistakes.
  - [[raw-borrow-operators]] — `&raw const`/`&raw mut` yangi sintaksis (Rust 2024); mutable static va packed struct uchun zarur.
  - [[unsafe-superpowers]] — 5 ta operatsiya overview.
  - [[safe-abstraction]] — pattern, `split_at_mut` misoli, anti-patternlar.
  - [[ffi]] — extern "C", safe keyword, ABI.
  - [[extern-block]] — `unsafe extern { ... }` deklaratsiya bloki.
  - [[mutable-static]] — `static mut`, atomic alternativalari, `static_mut_refs` lint.
  - [[unsafe-trait]] — `unsafe trait`/`unsafe impl`, Send/Sync misoli.
  - [[union-type]] — C union'lar, enum bilan taqqoslash.
  - [[undefined-behavior]] — UB manbalari, Miri bilan ushlash.
- Created [[miri]] tool sahifasi — UB detector, install, foydalanish, cheklovlar.
- Created [[abi]] glossary — Application Binary Interface qisqa ta'rifi.
- Created [[split-at-mut-unsafe]] example — Listing 20-4, 20-5, 20-6, 20-7 batafsil tahlili.
- Created [[e0133-call-to-unsafe-function]] — error sahifasi, fix patternlari.
- Updated [[unsafe-rust]] — eski stub kengaytirildi: 5 superpower jadvali, syntax, safe abstraction, SAFETY konventsiyasi, source_count 1→2.
- Updated [[static-items]] — mutable static, raw borrow operator havolalari qo'shildi; source_count 2→3.
- Updated [[send-trait]] — `unsafe impl Send` misoli, [[unsafe-trait]] havolasi; source_count 2→3.
- Updated [[sync-trait]] — `unsafe impl Sync` misoli, [[unsafe-trait]] havolasi; source_count 2→3.
- Updated [[index|Rust Wiki Index]] — 2 ta source, 2 ta chapter, 10 ta concept, 1 ta example, 1 ta error, 1 ta tool (Miri), 1 ta glossary (ABI), 1 ta snippet (option-take). source_count 93→95.
- Updated [[overview]] — Chapter 20 + 20.1 synthesis bloki qo'shildi; source_count 84→86.
- Bob 20 davom etadi: 20.2 Advanced traits, 20.3 Advanced types, 20.4 Advanced functions/closures, 20.5 Macros.

## 2026-05-08 ingest | 20.2. Advanced Traits

- Ingested `raw/books/the_rust_programming_language/20.2. Advanced Traits.md`.
- Created [[wiki/sources/20-2-advanced-traits]] — to'liq summary: associated types, default generic parameters, operator overloading, method disambiguation, fully qualified syntax, supertraits, newtype pattern.
- Created [[wiki/chapters/20-2-advanced-traits]] — Learning Goal, jadval, asosiy misollar, 14 ta concept havolasi, 6 mashq, 10 review savol.
- Created concept pages (6 ta):
  - [[associated-types]] — `type Item;` placeholder, generic bilan farq, qachon associated/qachon generic.
  - [[default-generic-parameters]] — `<Rhs = Self>`, ikki maqsad (backward-compat va customization), `Add` trait misoli.
  - [[operator-overloading]] — `std::ops` trait'lari, ruxsat etilgan operatorlar, custom Rhs.
  - [[fully-qualified-syntax]] — `<Type as Trait>::fn()`, method va associated fn farqi.
  - [[supertraits]] — `trait OutlinePrint: Display`, trait body'da supertrait method'larini ishlatish.
  - [[newtype-pattern]] — `struct Wrapper(Inner)`, orphan rule chetlab o'tish, type safety, Deref bilan transparent newtype.
- Created example pages (6 ta):
  - [[point-add-overload]] — Listing 20-15
  - [[millimeters-meters-add]] — Listing 20-16
  - [[human-pilot-wizard]] — Listing 20-17..20-19 (method disambiguation)
  - [[animal-dog-baby-name]] — Listing 20-20..20-22 (associated fn fully qualified)
  - [[outline-print-supertrait]] — Listing 20-23
  - [[wrapper-newtype-display]] — Listing 20-24
- Created [[e0790-cannot-call-associated-function]] — error sahifasi, `<Type as Trait>::fn()` fix.
- Updated [[traits]] — yangi 7 ta concept havolasi (associated types, default params, operator overloading, fully qualified syntax, supertraits, newtype pattern, unsafe trait); source_count 7→8.
- Updated [[orphan-rule]] — newtype pattern bo'limi qo'shildi (`Wrapper(Vec<String>)` misoli); source_count 1→2.
- Updated [[iterators]] — associated types havolasi, source_count 1→3 (20.2 source qo'shildi).
- Updated [[index|Rust Wiki Index]] — 1 ta source, 1 ta chapter, 6 ta concept, 6 ta example, 1 ta error qo'shildi; source_count 95→96.
- Updated [[overview]] — Chapter 20.2 synthesis bloki; source_count 86→87.
- Bob 20 davom etadi: 20.3 Advanced types, 20.4 Advanced functions/closures, 20.5 Macros.

## 2026-05-08 ingest | 20.3. Advanced Types

- Ingested `raw/books/the_rust_programming_language/20.3. Advanced Types.md`.
- Created [[wiki/sources/20-3-advanced-types]] — to'liq summary: newtype qayta ko'rib, type aliases (`Thunk`, `io::Result<T>`), never type `!`, DST (`str`, `[T]`, `dyn Trait`), `Sized` va `?Sized`.
- Created [[wiki/chapters/20-3-advanced-types]] — Learning Goal, jadval (alias vs newtype), asosiy misollar, 14 ta concept havolasi, 6 mashq, 10 review savol.
- Created concept pages (5 ta):
  - [[type-alias]] — `type Name = Existing`, newtype bilan jadval taqqoslash, `Thunk`, `io::Result`.
  - [[never-type]] — `!` empty type, match coercion, `unwrap` implementatsiyasi, `loop {}`.
  - [[diverging-functions]] — `-> !` qaytaruvchi funksiyalar, standart kutubxonadagi misollar (`panic!`, `process::exit`, `unreachable!`).
  - [[dynamically-sized-types]] — DST, fat pointer, oltin qoida (pointer orqali), `str`/`[T]`/`dyn Trait`.
  - [[sized-trait]] — auto-derived marker, generic implicit bound, `?Sized` syntax.
- Created example pages (2 ta):
  - [[thunk-type-alias]] — Listing 20-25, 20-26 + alias vs newtype taqqoslash.
  - [[io-result-alias]] — `Write` trait foydalanishi, alias method'lari saqlanishi.
- Updated [[newtype-pattern]] — type alias bilan jadval taqqoslash bo'limi qo'shildi; encapsulation foydalanishi qo'shildi; source_count 1→2.
- Updated [[string-slice]] — DST kontekstidagi havolalar; source_count 3→4.
- Updated [[trait-object]] — DST kontekstidagi havolalar; source_count 1→2.
- Updated [[loop]] — `never-type` havolasi (break'siz `!`); source_count 2→3.
- Updated [[match]] — `!` coercion havolasi; source_count 5→6.
- Updated [[unwrap]] — `!` coercion mexanizmi; source_count 2→3.
- Updated [[panic]] — `!` qaytarish; source_count 7→8.
- Updated [[generics]] — `Sized`, `?Sized`, `default-generic-parameters`, `associated-types` havolalari; source_count 7→8.
- Updated [[index|Rust Wiki Index]] — 1 source, 1 chapter, 5 concept, 2 example qo'shildi; source_count 96→97.
- Updated [[overview]] — Chapter 20.3 synthesis bloki; source_count 87→88.
- Bob 20 davom etadi: 20.4 Advanced functions/closures, 20.5 Macros.

## 2026-05-09 ingest | 20.4. Advanced Functions and Closures

- Ingested `raw/books/the_rust_programming_language/20.4. Advanced Functions and Closures.md`.
- Created [[wiki/sources/20-4-advanced-functions-and-closures]] — to'liq summary: `fn` function pointer, `Fn` traitlari bilan farq, `map(ToString::to_string)`, enum variant initializer, `impl Fn` return, opaque type mismatch, `Box<dyn Fn>` yechimi.
- Created [[wiki/chapters/20-4-advanced-functions-and-closures]] — Learning Goal, `fn` vs `Fn` jadvali, asosiy misollar, 13 concept havolasi, 6 mashq, 10 review savol.
- Created concept pages (3 ta):
  - [[function-pointers]] — lowercase `fn`, closure traitlari bilan farq, `F: Fn` API design, FFI context.
  - [[returning-closures]] — `impl Fn`, capture uchun `move`, `Box<dyn Fn>` bilan heterogeneous handlers.
  - [[opaque-types]] — return position `impl Trait`, alohida opaque typelar, E0308, trait object yechimi.
- Created example pages (3 ta):
  - [[function-pointer-do-twice]] — Listing 20-28.
  - [[map-with-named-functions]] — Listings 20-29, 20-30, 20-31.
  - [[boxed-closure-handlers]] — Listings 20-32, 20-33, 20-34.
- Updated [[index|Rust Wiki Index]] — 1 source, 1 chapter, 3 concept, 3 example qo'shildi; source_count 97→98.
- Updated [[overview]] — Chapter 20.4 synthesis bloki; source_count 88→89.
- Updated ontology file `infranodus/rust-book-relations.txt` — 20.4 function/closure relationlari qo'shildi.
- Bob 20 davom etadi: 20.5 Macros.

## 2026-05-09 ingest | 20.5. Macros

- Ingested `raw/books/the_rust_programming_language/20.5. Macros.md`.
- Created [[wiki/sources/20-5-macros]] — to'liq summary: macro vs function, declarative `macro_rules!`, simplified `vec!`, procedural macros, `TokenStream`, custom derive (`HelloMacro`), attribute-like (`route`), function-like (`sql!`).
- Created [[wiki/chapters/20-5-macros]] — Learning Goal, macro vs function jadvali, declarative/procedural macro sections, 13 concept havolasi, 3 example, 7 mashq, 11 review savol.
- Created concept pages (7 ta):
  - [[metaprogramming]] — code yozadigan code, compile-time expansion.
  - [[declarative-macros]] — `macro_rules!`, pattern/replacement, `vec!`.
  - [[procedural-macros]] — `TokenStream -> TokenStream`, `proc-macro` crate, `syn`, `quote`.
  - [[custom-derive-macros]] — `#[derive(HelloMacro)]`, trait impl generatsiyasi.
  - [[attribute-like-macros]] — `#[route(GET, "/")]`, `attr` va `item` token streams.
  - [[function-like-macros]] — `sql!(...)`, DSL-like token parsing.
  - [[tokenstream]] — procedural macro input/output tipi.
- Created example pages (3 ta):
  - [[simplified-vec-macro]] — Listing 20-35.
  - [[hello-macro-derive]] — Listings 20-37..20-42.
  - [[procedural-macro-forms]] — attribute-like `route` va function-like `sql!`.
- Updated [[macro]], [[vec-macro]], [[derive-attribute]], [[wiki/glossary/macros|macros glossary]] — 20.5 macro family havolalari qo'shildi.
- Updated [[index|Rust Wiki Index]] — 1 source, 1 chapter, 7 concept, 3 example qo'shildi; source_count 98→99.
- Updated [[overview]] — Chapter 20.5 synthesis bloki; source_count 89→90.
- Updated ontology file `infranodus/rust-book-relations.txt` — 20.5 macro relationlari qo'shildi.
- Chapter 20 ingest yakunlandi.

## 2026-05-13 ingest | 21 + 21.1 Web Server

- Ingested `raw/books/the_rust_programming_language/21. Final Project Building a Multithreaded Web Server.md`.
- Ingested `raw/books/the_rust_programming_language/21.1. Building a Single-Threaded Web Server.md`.
- Created [[wiki/sources/21-final-project-building-a-multithreaded-web-server]] — Chapter 21 intro summary: final project scope, TCP/HTTP plan, manual implementation motivation, thread pool va async runtime aloqasi.
- Created [[wiki/sources/21-1-building-a-single-threaded-web-server]] — to'liq summary: `TcpListener`, `TcpStream`, `BufReader`, request line, minimal response, `hello.html`, `404.html`, `Content-Length`, request-line routing, tuple refactor.
- Created [[wiki/chapters/21-final-project-building-a-multithreaded-web-server]] — chapter overview: project framing, low-level workflow, core concepts, examples, review savollar.
- Created [[wiki/chapters/21-1-building-a-single-threaded-web-server]] — step-by-step single-threaded server page: TCP vs HTTP, connection loop, request parsing, response writing, routing, bottleneck.
- Created concept pages (8 ta):
  - [[web-server]] — minimal Rust web server mental modeli.
  - [[tcp]] — reliable ordered byte stream transport.
  - [[http]] — request/response application protocol.
  - [[request-response]] — client-first interaction modeli.
  - [[tcp-listener]] — bind + incoming connections.
  - [[tcp-stream]] — read/write qilinadigan ochiq connection.
  - [[bufreader]] — buffered line-based reading helper.
  - [[content-length-header]] — response body byte length header.
- Created example pages (4 ta):
  - [[tcp-listener-connection-loop]] — Listing 21-1.
  - [[http-request-reader]] — Listing 21-2.
  - [[hello-html-response]] — Listings 21-3 va 21-5.
  - [[request-line-404-routing]] — Listings 21-6..21-9.
- Updated [[index|Rust Wiki Index]] — 2 source, 2 chapter, 8 concept, 4 example qo'shildi; source_count 99→101.
- Updated [[overview]] — Chapter 21 + 21.1 synthesis bloki; source_count 90→92.
- Updated ontology file `infranodus/rust-book-relations.txt` — Chapter 21 web server relationlari qo'shildi.
- Chapter 21 hozircha intro + 21.1 bilan boshlandi; next step concurrent handling va thread pool.

## 2026-05-13 ingest | 21.2 From Single-Threaded to Multithreaded Server

- Ingested `raw/books/the_rust_programming_language/21.2. From Single-Threaded to Multithreaded Server.md`.
- Created [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server]] — to'liq summary: `*/sleep*` bottleneck, per-request `thread::spawn`, fixed-size thread pool, compiler-driven development, `FnOnce + Send + 'static`, `Worker`, `mpsc`, `Arc<Mutex<Receiver<Job>>>`, `Job` alias, va `while let` lock lifetime pitfall.
- Created [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server]] — learning flow, evolution jadvali, design steps, review savollar, va amaliy mashqlar.
- Created concept pages (5 ta):
  - [[thread-pool]] — fixed-size worker pool va DoS/resource control.
  - [[worker-thread]] — `id + JoinHandle<()> + recv loop`.
  - [[job-queue]] — channel-backed job queue.
  - [[compiler-driven-development]] — desired API first, compiler feedback bilan implementation.
  - [[mutexguard-lifetime]] — `let` vs `while let` temporary lock scope farqi.
- Created example pages (6 ta):
  - [[slow-sleep-request]] — Listing 21-10.
  - [[per-request-thread-spawn]] — Listing 21-11.
  - [[threadpool-new-and-execute]] — Listing 21-12.
  - [[arc-mutex-mpsc-receiver]] — Listing 21-18.
  - [[job-boxed-fnonce-alias]] — Listings 21-19 va 21-20.
  - [[while-let-mutexguard-pitfall]] — Listing 21-21.
- Updated [[wiki/chapters/21-final-project-building-a-multithreaded-web-server]] — source_count 2→3, 21.2 subsection, new concepts/examples, va review savollar.
- Updated concept/error pages: [[web-server]], [[threads]], [[concurrency]], [[channels]], [[mutex-t|Mutex<T>]], [[arc-t|Arc<T>]], [[fn-traits|Fn traits]], [[send-trait|Send trait]], [[static-lifetime|static lifetime]], [[type-alias]], [[e0382-borrow-of-moved-value]].
- Updated [[index|Rust Wiki Index]] — 1 source, 1 chapter, 5 concept, 6 example qo'shildi; source_count 101→102.
- Updated [[overview]] — Chapter 21.2 synthesis bloki; source_count 92→93.
- Updated ontology file `infranodus/rust-book-relations.txt` — thread pool, worker, job queue, receiver sharing, va MutexGuard lifetime relationlari qo'shildi.
- Quick lint passed: new source/chapter indexga kiritildi; target concept/example fayllar yaratildi; ambiguous 21.2 source/chapter linklari path-qualified ishlatildi.

## 2026-05-13 ingest | 21.3 Graceful Shutdown and Cleanup

- Ingested `raw/books/the_rust_programming_language/21.3. Graceful Shutdown and Cleanup.md`.
- Created [[wiki/sources/21-3-graceful-shutdown-and-cleanup]] — to'liq summary: thread pool lifecycle, `Drop`, `join`, `E0507`, `Vec::drain(..)`, `Option::take()`, sender shutdown, `recv()` disconnect, va `incoming().take(2)` demo.
- Created [[wiki/chapters/21-3-graceful-shutdown-and-cleanup]] — learning goal, shutdown sequence, ownership nuances, exercises, va review savollar.
- Created concept/error pages:
  - [[graceful-shutdown]] — orderly thread pool cleanup va shutdown sequence.
  - [[e0507-cannot-move-out-of-borrowed-content]] — `JoinHandle::join(self)` cleanup kontekstidagi ownership xatosi.
- Created example pages (4 ta):
  - [[threadpool-drop-and-join]] — `Drop for ThreadPool` va worker `join()`.
  - [[sender-option-take-shutdown]] — `sender: Option<Sender<Job>>` + `drop(self.sender.take())`.
  - [[worker-recv-disconnect-shutdown]] — `recv()` `Err(_)` bo'lganda worker loop'dan chiqishi.
  - [[incoming-take-two-shutdown-demo]] — `listener.incoming().take(2)` bilan shutdown demo.
- Updated concept pages: [[thread-pool]], [[worker-thread]], [[drop]], [[join-handle]], [[channels]], [[option]], [[vector|Vec<T>]].
- Updated [[index|Rust Wiki Index]] — 1 source, 1 chapter, 1 concept, 4 example, 1 error qo'shildi; source_count 102→103.
- Updated [[overview]] — Chapter 21.3 cleanup/graceful-shutdown synthesis qo'shildi; source_count 93→94.
- Updated ontology file `infranodus/rust-book-relations.txt` — shutdown lifecycle, sender close, `recv` disconnect, `Option::take`, `Vec::drain`, va `join` relationlari append qilindi.
- Quick lint passed: frontmatter borligi, index/overview/log entrylari, 21.3 target fayllari, va ambiguous `21-3` wikilink yo'qligi `rg` bilan tekshirildi.

## 2026-05-13 ingest | 22 Appendix + 22.1 A - Keywords

- Ingested `raw/books/the_rust_programming_language/22. Appendix.md`.
- Ingested `raw/books/the_rust_programming_language/22.1. A - Keywords.md`.
- Created [[wiki/sources/22-appendix]] — appendix intro summary: reference layer, appendix sahifalarining vazifasi, va 22.1 ga ko'prik.
- Created [[wiki/sources/22-1-a-keywords]] — to'liq summary: current keywords, future reserved keywords, identifier ta'rifi, raw identifiers, `r#match`, va `r#try` edition compatibility.
- Created [[wiki/chapters/22-appendix]] — appendixning reference nature'i va 22.1 bilan bog'lanishi.
- Created [[wiki/chapters/22-1-a-keywords]] — keyword groups, raw identifier rule, edition compatibility, exercises, va review savollar.
- Created concept pages (2 ta):
  - [[identifiers]] — function, variable, module, crate, trait, lifetime va boshqa nomlash birliklari.
  - [[raw-identifiers]] — `r#keyword` syntax'i va cross-edition compatibility.
- Created example page:
  - [[raw-identifier-match-function]] — `fn match(...)` parse errori va `fn r#match(...)` yechimi.
- Updated [[keywords]] — source_count 1→2; current keywords, future reserved keywords, raw identifiers, va edition compatibility qo'shildi.
- Updated [[index|Rust Wiki Index]] — 2 source, 2 chapter, 2 concept, 1 example qo'shildi; source_count 103→105.
- Updated [[overview]] — Appendix 22 + 22.1 synthesis qo'shildi; source_count 94→96.
- Updated ontology file `infranodus/rust-book-relations.txt` — keywords, raw identifiers, identifiers, va edition compatibility relationlari append qilindi.
- Quick lint passed: frontmatter borligi, source/chapter index entrylari, `raw-identifiers` va `identifiers` targetlari, hamda ambiguous `22`/`22.1` wikilink yo'qligi `rg` bilan tekshirildi.

## 2026-05-13 ingest | 22.2 Operators and Symbols + 22.3 Derivable Traits

- Ingested `raw/books/the_rust_programming_language/22.2. B - Operators and Symbols.md`.
- Ingested `raw/books/the_rust_programming_language/22.3. C - Derivable Traits.md`.
- Created [[wiki/sources/22-2-b-operators-and-symbols]] — operator/symbol reference summary: overloadable operators, path/generic syntax, turbofish, raw strings, macros, comments, va bracket contexts.
- Created [[wiki/sources/22-3-c-derivable-traits]] — derivable traits summary: Debug, PartialEq, Eq, PartialOrd, Ord, Clone, Copy, Hash, Default, va `Display` nega derive qilinmasligi.
- Created [[wiki/chapters/22-2-b-operators-and-symbols]] — syntax clusters, key references, exercises, va review savollar.
- Created [[wiki/chapters/22-3-c-derivable-traits]] — trait families, practical rules, exercises, va review savollar.
- Created concept pages (8 ta):
  - [[rust-operators-and-symbols]] — syntax atlas overview.
  - [[turbofish]] — `::<...>` expression-context generics syntax.
  - [[raw-string-literals]] — `r"..."`, `r#"..."#`, raw byte string familyasi.
  - [[partial-eq]] — equality comparisons va `assert_eq!`.
  - [[eq-trait]] — reflexive equality marker trait.
  - [[ord-trait]] — total ordering va `cmp`.
  - [[hash-trait]] — hash-based key semantics.
  - [[default-trait]] — default values, `..Default::default()`, `unwrap_or_default`.
- Created example pages (2 ta):
  - [[turbofish-parse-example]] — `"42".parse::<i32>()`.
  - [[derive-debug-partialeq-assert-eq]] — `#[derive(Debug, PartialEq)]` va `assert_eq!`.
- Updated appendix synthesis page [[wiki/chapters/22-appendix]] — source_count 2→4; 22.2 va 22.3 reference scope qo'shildi.
- Updated concept pages: [[operator-overloading]], [[derive-attribute]], [[debug-trait]], [[clone]], [[copy-trait]], [[partial-ord]], [[question-mark-operator]], [[dereference-operator]], [[raw-borrow-operators]], [[comments]], [[macro]].
- Updated [[index|Rust Wiki Index]] — 2 source, 2 chapter, 8 concept, 2 example qo'shildi; source_count 105→107.
- Updated [[overview]] — Appendix 22.2 + 22.3 synthesis qo'shildi; source_count 96→98.
- Updated ontology file `infranodus/rust-book-relations.txt` — operator/symbol va derivable trait relationlari append qilindi.
- Quick lint passed: frontmatter, source/chapter index entrylari, yangi concept/example targetlari, va ambiguous `22-2`/`22-3` wikilink yo'qligi `rg` bilan tekshirildi.

## 2026-05-13 ingest | 22.4 Tools + 22.5 Editions + 22.6 Translations + 22.7 Nightly Rust

- Ingested 4 ta source:
  - `raw/books/the_rust_programming_language/22.4. D - Useful Development Tools.md`
  - `raw/books/the_rust_programming_language/22.5. E - Editions.md`
  - `raw/books/the_rust_programming_language/22.6. F - Translations of the Book.md`
  - `raw/books/the_rust_programming_language/22.7. G - How Rust is Made and “Nightly Rust”.md`
- Created source summaries:
  - [[wiki/sources/22-4-d-useful-development-tools]]
  - [[wiki/sources/22-5-e-editions]]
  - [[wiki/sources/22-6-f-translations-of-the-book]]
  - [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust]]
- Created chapter pages:
  - [[wiki/chapters/22-4-d-useful-development-tools]]
  - [[wiki/chapters/22-5-e-editions]]
  - [[wiki/chapters/22-6-f-translations-of-the-book]]
  - [[wiki/chapters/22-7-g-how-rust-is-made-and-nightly-rust]]
- Created tool pages:
  - [[clippy]]
  - [[cargo-fix|cargo fix / rustfix]]
  - [[rust-analyzer]]
- Created concept pages:
  - [[rust-editions]]
  - [[edition-migration]]
  - [[cargo-fix-edition-migration]]
  - [[rust-book-translations]]
  - [[nightly-rust]]
  - [[release-channels]]
  - [[feature-flags]]
  - [[rust-rfc-process]]
  - [[stability-without-stagnation]]
- Created example pages:
  - [[cargo-fix-unused-mut-example]]
  - [[clippy-approx-constant-example]]
  - [[rustup-nightly-override-example]]
- Updated existing pages:
  - [[rustfmt]], [[cargo|Cargo]], [[tooling]], [[rust-language-server|Rust Language Server]], [[rustup]], [[miri|Miri]], [[compiler]]
  - [[readable-code]], [[testing]], [[edition]], [[rust-2024-edition|Rust 2024 Edition]]
  - [[wiki/chapters/22-appendix]]
- Updated [[index|Rust Wiki Index]] — 4 source, 4 chapter, 3 tool, 9 concept, 3 example qo'shildi; source_count 107→111.
- Updated [[overview]] — Appendix 22.4–22.7 synthesis qo'shildi; source_count 98→102.
- Updated ontology file `infranodus/rust-book-relations.txt` — tooling, editions, translations snapshot, release channels, nightly, va RFC process relationlari append qilindi.
- Quick lint passed: frontmatter borligi, index/source entrylari, yangi wikilink targetlari, va ambiguous `22-4`/`22-5`/`22-6`/`22-7` wikilink yo'qligi `rg` bilan tekshirildi.

## 2026-05-13 lint | Full wiki lint

- Ran full lint across `wiki/` and `raw/`: 654 wiki pages, 111 raw source files, 111 source summaries, 10087 wikilinks.
- Fixed 3 broken release-channel wikilinks in [[compiler]]: `stable`/`beta` -> [[release-channels]], `nightly` -> [[nightly-rust]].
- Updated [[index|Rust Wiki Index]] with 13 missing content pages: [[correctness]], [[interior-mutability]], [[memory-leak]], [[rc-t]], [[refcell-t]], [[reference-cycle]], [[weak-t]], [[rc-shared-list]], [[tree-with-weak-parent]], [[e0005-refutable-pattern-in-local-binding]], [[e0040-explicit-use-of-destructor]], [[destructor]], [[double-free]].
- Updated [[overview]] source inventory: source_count 102 -> 111; added 15.3-16.4 source links.
- Saved report: `output/2026-05-13-wiki-lint-report.md`.
- Final lint state: 0 broken links, 0 ambiguous links, 0 orphan pages, 0 frontmatter issues, 0 raw/source coverage gaps, 0 index coverage gaps.
- Remaining intentional `needs-review`: [[wiki/sources/17-4-streams-futures-in-sequence]], [[wiki/chapters/17-4-streams-futures-in-sequence]], and [[stream]] because raw 17.4 is partial.

## 2026-05-16 ingest | Rust for Backend Developers initial batch

- Ingested 7 ta source from `raw/books/rust-for-backend-developer/`:
  - `1. Предисловие.md`
  - `2. Установка.md`
  - `3. Установка на Windows.md`
  - `4. Установка на Linux.md`
  - `5. Среда разработки.md`
  - `6. Первый взгляд.md`
  - `7. Безопасный Rust.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-preface]]
  - [[wiki/sources/rust-for-backend-developers-setup-rust]]
  - [[wiki/sources/rust-for-backend-developers-install-on-windows]]
  - [[wiki/sources/rust-for-backend-developers-install-on-linux]]
  - [[wiki/sources/rust-for-backend-developers-development-environment]]
  - [[wiki/sources/rust-for-backend-developers-first-look]]
  - [[wiki/sources/rust-for-backend-developers-safe-rust]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-preface]]
  - [[wiki/chapters/rust-for-backend-developers-setup-rust]]
  - [[wiki/chapters/rust-for-backend-developers-install-on-windows]]
  - [[wiki/chapters/rust-for-backend-developers-install-on-linux]]
  - [[wiki/chapters/rust-for-backend-developers-development-environment]]
  - [[wiki/chapters/rust-for-backend-developers-first-look]]
  - [[wiki/chapters/rust-for-backend-developers-safe-rust]]
- Created concept/tool pages:
  - [[c-toolchain|C/C++ toolchain]]
  - [[msvc-toolchain|MSVC toolchain]]
  - [[mingw|MinGW]]
  - [[language-server-protocol|Language Server Protocol]]
  - [[vscode|VSCode]], [[zed-editor|Zed]], [[rust-rover|Rust Rover]], [[neovim|NeoVim]], [[helix-editor|Helix]]
- Updated existing pages: [[rustup]], [[rustc]], [[cargo|Cargo]], [[rust-analyzer]], [[tooling]], [[linker]], [[memory-safety]], [[unsafe-rust]], [[data-race]], [[undefined-behavior]], [[main-function]], [[println-macro]], [[hello-world]].
- Updated [[index|Rust Wiki Index]] source_count 111->118 and added new source/chapter/concept/tool entries.
- Updated [[overview]] source_count 111->118 and added backend-developer source synthesis.
- Updated ontology file `infranodus/rust-book-relations.txt` with backend setup, IDE/LSP, first-look, and safe Rust relations.
- Targeted quick lint passed: 7 source summaries exist, new pages have required frontmatter, new/touched page wikilinks have 0 broken and 0 ambiguous targets, and backend source/chapter same-basename links are path-qualified.

## 2026-05-16 refactor | Rust for Backend Developers `1. intro`

- Raw source joylashuvi `raw/books/rust-for-backend-developer/1. intro/` ga ko'chgani hisobga olindi.
- Updated 7 ta source summary ichidagi `Raw:` pathlar yangi `1. intro/` joylashuviga moslandi.
- Created section chapter [[wiki/chapters/rust-for-backend-developers-1-intro]] — intro source'larini bitta onboarding section sifatida bog'laydi.
- Updated [[index|Rust Wiki Index]] va [[overview]] — Rust for Backend Developers materiallari endi `1. intro` section sifatida aniq ko'rsatiladi.
- Existing source/chapter sluglari saqlab qolindi; sabab: hozirgi wikilinklarni sindirmaslik va minimal-risk refactor qilish.

## 2026-05-16 ingest | Rust for Backend Developers `2. base`

- Ingested 8 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `8. Переменные.md`
  - `9. Примитивные типы данных.md`
  - `10. Печать на консоль.md`
  - `11. Скоупы.md`
  - `12. Ссылки.md`
  - `13. Массивы.md`
  - `14. Вектор.md`
  - `15. Слайсы.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-variables]]
  - [[wiki/sources/rust-for-backend-developers-primitive-types]]
  - [[wiki/sources/rust-for-backend-developers-console-output]]
  - [[wiki/sources/rust-for-backend-developers-scopes]]
  - [[wiki/sources/rust-for-backend-developers-references]]
  - [[wiki/sources/rust-for-backend-developers-arrays]]
  - [[wiki/sources/rust-for-backend-developers-vector]]
  - [[wiki/sources/rust-for-backend-developers-slices]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-2-base]]
  - [[wiki/chapters/rust-for-backend-developers-variables]]
  - [[wiki/chapters/rust-for-backend-developers-primitive-types]]
  - [[wiki/chapters/rust-for-backend-developers-console-output]]
  - [[wiki/chapters/rust-for-backend-developers-scopes]]
  - [[wiki/chapters/rust-for-backend-developers-references]]
  - [[wiki/chapters/rust-for-backend-developers-arrays]]
  - [[wiki/chapters/rust-for-backend-developers-vector]]
  - [[wiki/chapters/rust-for-backend-developers-slices]]
- Created concept pages:
  - [[type-casting]]
  - [[discarded-binding]]
- Updated concept pages: [[variables-and-mutability|variables and mutability]], [[immutability]], [[constants]], [[static-items]], [[raw-identifiers]], [[scalar-types|scalar types]], [[unit-type|unit type]], [[never-type|never type (!)]], [[println-macro|println! macro]], [[display-formatting|Display formatting]], [[debug-formatting|Debug formatting]], [[scope]], [[statements-and-expressions|statements and expressions]], [[reference]], [[borrowing]], [[mutable-reference|mutable reference]], [[array]], [[vector|Vec<T>]], [[vec-macro|vec! macro]], [[slices]].
- Updated [[index|Rust Wiki Index]] source_count 118->126 and added 8 source, 9 chapter, and 2 concept entries.
- Updated [[overview]] source_count 118->126 and added `2. base` source inventory plus section synthesis.
- Updated ontology file `infranodus/rust-book-relations.txt` with variables, formatting, reference, vector, and slice relations.
- Minor lint fix: historical broken log wikilinks retarget qilindi (`drop-trait` -> [[drop]], `wiki/index.md` -> [[index]], `mpsc` -> [[channels]]).

## 2026-05-16 ingest | Rust for Backend Developers `2. base` 16-20

- Ingested 5 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `16. Строки.md`
  - `17. Условный оператор if.md`
  - `18. Циклы.md`
  - `19. Функции.md`
  - `20. Кортежи.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-strings]]
  - [[wiki/sources/rust-for-backend-developers-if]]
  - [[wiki/sources/rust-for-backend-developers-loops]]
  - [[wiki/sources/rust-for-backend-developers-functions]]
  - [[wiki/sources/rust-for-backend-developers-tuples]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-strings]]
  - [[wiki/chapters/rust-for-backend-developers-if]]
  - [[wiki/chapters/rust-for-backend-developers-loops]]
  - [[wiki/chapters/rust-for-backend-developers-functions]]
  - [[wiki/chapters/rust-for-backend-developers-tuples]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 8->13, strings/control-flow/functions/tuples synthesis qo'shildi.
- Updated concept/glossary pages: [[string-type|String]], [[string-slice|String slice]], [[format-macro|format! macro]], [[utf-8|UTF-8]], [[slices]], [[if-expressions|if expressions]], [[statements-and-expressions|statements and expressions]], [[scope]], [[unit-type|unit type]], [[while-loop|while loop]], [[loop]], [[for-loop|for loop]], [[functions]], [[tuple]], [[never-type|never type (!)]], [[wiki/glossary/string-literal|String Literal]], [[wiki/glossary/range|Range]], [[wiki/glossary/break|break]], [[wiki/glossary/continue|continue]].
- Updated [[index|Rust Wiki Index]] va [[overview]] source_count 126->131; 5 source va 5 chapter link qo'shildi.
- Appended new `2. base` string/control-flow/function/tuple relations to `infranodus/rust-book-relations.txt` without regenerating existing ontology.

## 2026-05-16 ingest | Rust for Backend Developers `2. base` 21-22

- Ingested 2 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `21. Владение.md`
  - `22. Лайфтаймы.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-ownership]]
  - [[wiki/sources/rust-for-backend-developers-lifetimes]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-ownership]]
  - [[wiki/chapters/rust-for-backend-developers-lifetimes]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 13->15; ownership, borrowing, borrow checker, lifetimes, `'static` synthesis qo'shildi.
- Updated concept/error pages: [[ownership]], [[move-semantics|move semantics]], [[drop]], [[borrow-checker]], [[lifetimes]], [[static-lifetime]], [[dangling-reference|dangling reference]], [[e0106-missing-lifetime-specifier|E0106]], [[e0382-borrow-of-moved-value|E0382]], [[e0499-multiple-mutable-borrows|E0499]], [[e0502-mutable-and-immutable-borrow-conflict|E0502]].
- Updated [[index|Rust Wiki Index]] va [[overview]] source_count 131->133; 2 source va 2 chapter link qo'shildi.
- Appended ownership/lifetime relations to `infranodus/rust-book-relations.txt`; existing ontology qayta generatsiya qilinmadi.
- Source claim correction: ownership cleanup va referential safety kuchli, lekin "safe Rust'da leaks mutlaqo imkonsiz" degan talqin ehtiyotkorroq yozildi.

## 2026-05-16 ingest | Rust for Backend Developers `2. base` 23-24

- Ingested 2 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `23. Декларативные макросы.md`
  - `24. Указатели.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-declarative-macros]]
  - [[wiki/sources/rust-for-backend-developers-pointers]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-declarative-macros]]
  - [[wiki/chapters/rust-for-backend-developers-pointers]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 15->17; `macro_rules!`, repetition, raw pointers, unsafe, dereference, va Miri synthesis qo'shildi.
- Updated concept/example/tool pages: [[macro]], [[declarative-macros|declarative macros]], [[procedural-macros|procedural macros]], [[vec-macro|vec! macro]], [[simplified-vec-macro]], [[statements-and-expressions|statements and expressions]], [[pattern-matching]], [[identifiers]], [[raw-pointer]], [[raw-borrow-operators|raw borrow operators]], [[dereference-operator|dereference operator]], [[unsafe-rust|unsafe Rust]], [[unsafe-superpowers|unsafe superpowers]], [[reference]], [[mutable-reference|mutable reference]], [[borrowing]], [[data-race|data race]], [[undefined-behavior|undefined behavior]], [[miri|Miri]].
- Updated [[index|Rust Wiki Index]] va [[overview]] source_count 133->135; 2 source va 2 chapter link qo'shildi.
- Appended macro/pointer relations to `infranodus/rust-book-relations.txt`; ontology qayta generatsiya qilinmadi.
- Source claim correction: pointer chapterdagi "unsafe fn body to'liq unsafe block" talqini wiki'da ehtiyotkor yozildi; Rust 2024 semanticsida explicit `unsafe {}` scope afzal va ko'pincha kerak.

## 2026-05-16 ingest | Rust for Backend Developers `2. base` 25-26

- Ingested 2 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `25. Структуры.md`
  - `26. Модули.md`
- Created source summaries:
  - [[wiki/sources/rust-for-backend-developers-structs]]
  - [[wiki/sources/rust-for-backend-developers-modules]]
- Created chapter pages:
  - [[wiki/chapters/rust-for-backend-developers-structs]]
  - [[wiki/chapters/rust-for-backend-developers-modules]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 17->19; structs, `impl`, methods, tuple/unit-like structs, `mod`, `pub`, `use`, module file layout, va crate mental modeli qo'shildi.
- Updated concept pages: [[structs]], [[struct-fields|struct fields]], [[struct-instances|struct instances]], [[field-init-shorthand]], [[struct-update-syntax]], [[methods]], [[impl-block|impl block]], [[associated-functions]], [[tuple-structs]], [[unit-like-structs]], [[lifetimes]], [[module-system|module system]], [[module]], [[pub-keyword|pub keyword]], [[use-declarations|use declarations]], [[mod-declarations|mod declarations]], [[module-file-layout|module file layout]], [[crate]], [[crate-root|crate root]].
- Updated [[index|Rust Wiki Index]] va [[overview]] source_count 135->137; 2 source va 2 chapter link qo'shildi.
- Appended struct/module relations to `infranodus/rust-book-relations.txt`; ontology qayta generatsiya qilinmadi.
- Source caveat preservation: modules chapterdagi "hamma fayllar go'yo bitta katta fayl" modeli wiki'da literal compiler implementation sifatida emas, beginner mental model sifatida yozildi. Struct layout bo'limidagi aniq size/address natijalari esa platforma/compilerga bog'liq deb qayd qilindi.

## 2026-05-16 ingest | Rust for Backend Developers `2. base` 27

- Ingested 1 ta source from `raw/books/rust-for-backend-developer/2. base/`:
  - `27. Трэйты.md`
- Created source summary:
  - [[wiki/sources/rust-for-backend-developers-traits]]
- Created chapter page:
  - [[wiki/chapters/rust-for-backend-developers-traits]]
- Created concept pages:
  - [[static-dispatch|static dispatch]]
  - [[dyn-compatibility|dyn compatibility]]
- Updated section chapter [[wiki/chapters/rust-for-backend-developers-2-base]]: source_count 19->20; traits, polymorphism, static/dynamic dispatch, trait objects, orphan rule, default implementations, supertraits, `Self`, dyn compatibility, va `unsafe trait` qo'shildi.
- Updated concept pages: [[traits]], [[trait-definitions|trait definitions]], [[trait-implementations|trait implementations]], [[impl-trait|impl Trait]], [[trait-object|trait object]], [[dynamic-dispatch|dynamic dispatch]], [[polymorphism]], [[monomorphization]], [[orphan-rule|orphan rule]], [[newtype-pattern|newtype pattern]], [[default-trait-implementations|default trait implementations]], [[supertraits]], [[trait-bounds|trait bounds]], [[self-type|Self type]], [[box-t|Box<T>]], [[sized-trait|Sized trait]], [[unsafe-trait|unsafe trait]], [[send-trait|Send trait]], [[sync-trait|Sync trait]].
- Updated [[index|Rust Wiki Index]] va [[overview]] source_count 137->138; 1 source va 1 chapter link qo'shildi.
- Appended trait relations to `infranodus/rust-book-relations.txt`; ontology qayta generatsiya qilinmadi.
- Source caveat preservation: trait object cheklovlari wiki'da beginner simplification sifatida yozildi; source associated typesni umumiy taqiq sifatida soddalashtiradi, wiki esa bunu dyn compatibility modeli bilan ehtiyotkor tushuntirdi.
