---
title: "Rust Learning Roadmap"
type: roadmap
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, roadmap]
source_count: 43
---

# Rust Learning Roadmap

## Current Stage

Rust Book opening materials through Chapter 10.2 are ingested. Current practical baseline: install/update with [[rustup]], verify compile toolchain with [[rustc]], write [[hello-world]], use [[cargo|Cargo]], build [[guessing-game]], understand common Rust programming concepts, explain [[ownership]], [[borrowing]], [[reference|references]], [[slices]], model related data with [[structs]], attach behavior with [[methods]], define [[enums]] with [[option|Option<T>]] as the null replacement, handle enum values with [[match]], [[if-let|if let]], and [[let-else|let...else]], distinguish [[package]], [[crate]], [[binary-crate|binary crate]], [[library-crate|library crate]], and [[crate-root|crate root]], explain module-system relationships through Chapter 7, use basic [[collections|collections]] concepts through Chapter 8, explain Chapter 9 error-handling basics, and use Chapter 10.2 abstraction basics: [[generics]], [[generic-type-parameters|generic type parameters]], [[generic-functions|generic functions]], [[generic-structs|generic structs]], [[generic-enums|generic enums]], [[generic-methods|generic methods]], [[partial-ord|PartialOrd]], [[monomorphization]], [[traits]], [[trait-definitions|trait definitions]], [[trait-implementations|trait implementations]], [[trait-bounds|trait bounds]], [[impl-trait|impl Trait]], [[where-clauses|where clauses]], [[orphan-rule|orphan rule]], [[blanket-implementations|blanket implementations]], [[conditional-method-implementations|conditional method implementations]], [[lifetimes]], [[code-duplication|code duplication]], [[function-extraction|function extraction]], and [[largest-function-extraction|largest function extraction]].

Learning strategy from [[wiki/chapters/0-2-introduction|0.2. Introduction]]:

- Read mostly in sequence because later chapters build on earlier concepts.
- Use project chapters to apply concepts: Chapter 2, Chapter 12, and Chapter 21.
- Save recurring compiler diagnostics into `wiki/errors/`.

## Core Track

- [x] Setup: `rustup`, `cargo`, editor tooling
- [x] Syntax and basic types
- [x] Variables and mutability
- [x] Match and Result basics
- [x] Functions
- [x] Comments
- [x] Control flow
- [x] Ownership
- [x] Borrowing and references
- [x] Slices
- [x] Structs
- [x] Methods and associated functions
- [x] Enums
- [~] Pattern matching (Chapter 6 basics done; deeper patterns Chapter 19)
- [x] Error handling (Chapter 9 complete: panic, Result, propagation, and panic-vs-Result decision criteria)
- [~] Generics (Chapter 10 opening and 10.1 generic functions, structs, enums, methods, and monomorphization done; later generic patterns pending)
- [~] Traits (Chapter 10.2 trait definitions, implementations, bounds, impl Trait, where clauses, orphan rule, defaults, conditional methods, and blanket impls done; deeper trait objects later)
- [~] Lifetimes (Chapter 10 opening frames lifetimes as generics for reference relationships; 10.3 detailed lifetime syntax pending)
- [x] Collections (Chapter 8 vectors, strings, and hash maps done)
- [ ] Iterators and closures
- [x] Modules and crates (Chapter 7 package/crate, module tree/privacy, paths, `super`, public structs/enums, `use`, `as` aliases, external packages, glob operator, `pub use`, and module file layout done)
- [ ] Testing
- [ ] Smart pointers
- [ ] Concurrency
- [ ] Async Rust
- [ ] Unsafe Rust

## Practice Track

- [x] Hello, world!
- [x] Guessing game
- [ ] Chapter 3 practice: converter, Fibonacci, lyrics
- [ ] Ownership move/clone/copy drills
- [ ] Small CLI project
- [ ] File parser
- [ ] Error-handling refactor
- [ ] Trait-based abstraction
- [ ] Async API client
- [ ] Multithreaded web server
