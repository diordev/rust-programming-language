---
title: "Rust Wiki Overview"
type: overview
status: active
created: 2026-05-06
updated: 2026-05-19
tags: [rust, overview]
source_count: 170
---

# Rust Wiki Overview

This wiki is a living Rust learning reference. It will grow from book chapters, personal notes, examples, exercises, and questions.

Current focus:

- Build a durable mental model of Rust.
- Keep Rust terms in English while explaining in Uzbek.
- Connect chapters to reusable concept pages.
- Save useful answers back into the wiki so knowledge compounds.

Ingested Rust Book materials:

- [[0-the-rust-programming-language|0. The Rust Programming Language]]
- [[0-1-foreword|0.1. Foreword]]
- [[wiki/sources/0-2-introduction|0.2. Introduction]]
- [[wiki/sources/1-getting-started|1. Getting Started]]
- [[1-1-installation|1.1. Installation]]
- [[1-2-hello-world|1.2. Hello, World!]]
- [[1-3-hello-cargo|1.3. Hello, Cargo!]]
- [[wiki/sources/2-programming-a-guessing-game|2. Programming a Guessing Game]]
- [[wiki/sources/3-common-programming-concepts|3. Common Programming Concepts]]
- [[3-1-variables-and-mutability|3.1. Variables and Mutability]]
- [[3-2-data-types|3.2. Data Types]]
- [[3-3-functions|3.3. Functions]]
- [[3-4-comments|3.4. Comments]]
- [[3-5-control-flow|3.5. Control Flow]]
- [[wiki/sources/4-understanding-ownership|4. Understanding Ownership]]
- [[4-1-what-is-ownership|4.1. What Is Ownership?]]
- [[4-2-references-and-borrowing|4.2. References and Borrowing]]
- [[4-3-the-slice-type|4.3. The Slice Type]]
- [[wiki/sources/5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]]
- [[5-1-defining-and-instantiating-structs|5.1. Defining and Instantiating Structs]]
- [[5-2-an-example-program-using-structs|5.2. An Example Program Using Structs]]
- [[5-3-methods|5.3. Methods]]
- [[wiki/sources/6-enums-and-pattern-matching|6. Enums and Pattern Matching]]
- [[6-1-defining-an-enum|6.1. Defining an Enum]]
- [[6-2-the-match-control-flow-construct|6.2. The match Control Flow Construct]]
- [[6-3-concise-control-flow-with-if-let-and-let-else|6.3. Concise Control Flow with if let and let...else]]
- [[wiki/sources/7-packages-crates-and-modules|7. Packages, Crates, and Modules]]
- [[7-1-packages-and-crates|7.1. Packages and Crates]]
- [[7-2-control-scope-and-privacy-with-modules|7.2. Control Scope and Privacy with Modules]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree|7.3. Paths for Referring to an Item in the Module Tree]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword|7.4. Bringing Paths Into Scope with the use Keyword]]
- [[7-5-separating-modules-into-different-files|7.5. Separating Modules into Different Files]]
- [[wiki/sources/8-common-collections|8. Common Collections]]
- [[8-1-storing-lists-of-values-with-vectors|8.1. Storing Lists of Values with Vectors]]
- [[8-2-storing-utf-8-encoded-text-with-strings|8.2. Storing UTF-8 Encoded Text with Strings]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps|8.3. Storing Keys with Associated Values in Hash Maps]]
- [[wiki/sources/9-error-handling|9. Error Handling]]
- [[9-1-unrecoverable-errors-with-panic|9.1. Unrecoverable Errors with panic!]]
- [[9-2-recoverable-errors-with-result|9.2. Recoverable Errors with Result]]
- [[9-3-to-panic-or-not-to-panic|9.3. To panic! or Not to panic!]]
- [[wiki/sources/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]]
- [[10-1-generic-data-types|10.1. Generic Data Types]]
- [[10-2-defining-shared-behavior-with-traits|10.2. Defining Shared Behavior with Traits]]
- [[10-3-validating-references-with-lifetimes|10.3. Validating References with Lifetimes]]
- [[wiki/sources/11-writing-automated-tests|11. Writing Automated Tests]]
- [[11-1-how-to-write-tests|11.1. How to Write Tests]]
- [[11-2-controlling-how-tests-are-run|11.2. Controlling How Tests Are Run]]
- [[11-3-test-organization|11.3. Test Organization]]
- [[12-an-io-project-building-a-command-line-program|12. An I/O Project: Building a Command Line Program]]
- [[12-1-accepting-command-line-arguments|12.1. Accepting Command Line Arguments]]
- [[12-2-reading-a-file|12.2. Reading a File]]
- [[12-3-refactoring-to-improve-modularity-and-error-handling|12.3. Refactoring to Improve Modularity and Error Handling]]
- [[12-4-adding-functionality-with-test-driven-development|12.4. Adding Functionality with Test-Driven Development]]
- [[12-5-working-with-environment-variables|12.5. Working with Environment Variables]]
- [[12-6-redirecting-errors-to-standard-error|12.6. Redirecting Errors to Standard Error]]
- [[13-functional-language-features-iterators-and-closures|13. Functional Language Features: Iterators and Closures]]
- [[13-1-closures|13.1. Closures]]
- [[13-2-processing-a-series-of-items-with-iterators|13.2. Processing a Series of Items with Iterators]]
- [[13-3-improving-our-io-project|13.3. Improving Our I/O Project]]
- [[13-4-performance-in-loops-vs-iterators|13.4. Performance: Comparing Loops and Iterators]]
- [[wiki/sources/14-more-about-cargo-and-crates-io|14. More About Cargo and Crates.io]]
- [[14-1-customizing-builds-with-release-profiles|14.1. Customizing Builds with Release Profiles]]
- [[14-2-publishing-a-crate-to-crates-io|14.2. Publishing a Crate to Crates.io]]
- [[14-3-cargo-workspaces|14.3. Cargo Workspaces]]
- [[14-4-installing-binaries-with-cargo-install|14.4. Installing Binaries with cargo install]]
- [[14-5-extending-cargo-with-custom-commands|14.5. Extending Cargo with Custom Commands]]
- [[wiki/sources/15-smart-pointers|15. Smart Pointers]]
- [[15-1-using-box-t-to-point-to-data-on-the-heap|15.1. Using Box<T> to Point to Data on the Heap]]
- [[15-2-treating-smart-pointers-like-regular-references|15.2. Treating Smart Pointers Like Regular References]]
- [[15-3-running-code-on-cleanup-with-the-drop-trait|15.3. Running Code on Cleanup with the Drop Trait]]
- [[15-4-rc-the-reference-counted-smart-pointer|15.4. Rc<T>, the Reference-Counted Smart Pointer]]
- [[15-5-refcell-t-and-the-interior-mutability-pattern|15.5. RefCell<T> and the Interior Mutability Pattern]]
- [[15-6-reference-cycles-can-leak-memory|15.6. Reference Cycles Can Leak Memory]]
- [[wiki/sources/16-fearless-concurrency|16. Fearless Concurrency]]
- [[16-1-using-threads-to-run-code-simultaneously|16.1. Using Threads to Run Code Simultaneously]]
- [[16-2-transfer-data-between-threads-with-message-passing|16.2. Transfer Data Between Threads with Message Passing]]
- [[16-3-shared-state-concurrency|16.3. Shared-State Concurrency]]
- [[16-4-extensible-concurrency-with-send-and-sync|16.4. Extensible Concurrency with Send and Sync]]
- [[17-fundamentals-of-asynchronous-programming|17. Fundamentals of Asynchronous Programming]]
- [[17-1-futures-and-the-async-syntax|17.1. Futures and the Async Syntax]]
- [[wiki/sources/17-2-applying-concurrency-with-async|17.2. Applying Concurrency with Async]]
- [[wiki/sources/17-3-working-with-any-number-of-futures|17.3. Working With Any Number of Futures]]
- [[wiki/sources/17-4-streams-futures-in-sequence|17.4. Streams: Futures in Sequence]]
- [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async|17.5. A Closer Look at the Traits for Async]]
- [[wiki/sources/17-6-futures-tasks-and-threads|17.6. Futures, Tasks, and Threads]]
- [[18-object-oriented-programming-features|18. Object Oriented Programming Features]]
- [[wiki/sources/18-1-characteristics-of-object-oriented-languages|18.1. Characteristics of Object-Oriented Languages]]
- [[18-2-using-trait-objects-to-abstract-over-shared-behavior|18.2. Using Trait Objects to Abstract over Shared Behavior]]
- [[wiki/sources/18-3-implementing-an-object-oriented-design-pattern|18.3. Implementing an Object-Oriented Design Pattern]]
- [[wiki/sources/19-patterns-and-matching|19. Patterns and Matching]]
- [[wiki/sources/19-1-all-the-places-patterns-can-be-used|19.1. All the Places Patterns Can Be Used]]
- [[wiki/sources/19-2-refutability-whether-a-pattern-might-fail-to-match|19.2. Refutability: Whether a Pattern Might Fail to Match]]
- [[wiki/sources/19-3-pattern-syntax|19.3. Pattern Syntax]]
- [[wiki/sources/20-advanced-features|20. Advanced Features]]
- [[wiki/sources/20-1-unsafe-rust|20.1. Unsafe Rust]]
- [[wiki/sources/20-2-advanced-traits|20.2. Advanced Traits]]
- [[wiki/sources/20-3-advanced-types|20.3. Advanced Types]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4. Advanced Functions and Closures]]
- [[wiki/sources/20-5-macros|20.5. Macros]]
- [[wiki/sources/21-final-project-building-a-multithreaded-web-server|21. Final Project: Building a Multithreaded Web Server]]
- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1. Building a Single-Threaded Web Server]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2. From Single-Threaded to Multithreaded Server]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3. Graceful Shutdown and Cleanup]]
- [[wiki/sources/22-appendix|22. Appendix]]
- [[wiki/sources/22-1-a-keywords|22.1. A - Keywords]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2. B - Operators and Symbols]]
- [[wiki/sources/22-3-c-derivable-traits|22.3. C - Derivable Traits]]
- [[wiki/sources/22-4-d-useful-development-tools|22.4. D - Useful Development Tools]]
- [[wiki/sources/22-5-e-editions|22.5. E - Editions]]
- [[wiki/sources/22-6-f-translations-of-the-book|22.6. F - Translations of the Book]]
- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7. G - How Rust is Made and Nightly Rust]]

Ingested Rust for Backend Developers materials (`1. intro` so far):

- [[wiki/sources/rust-for-backend-developers-preface|Preface]]
- [[wiki/sources/rust-for-backend-developers-setup-rust|Setup Rust]]
- [[wiki/sources/rust-for-backend-developers-install-on-windows|Install on Windows]]
- [[wiki/sources/rust-for-backend-developers-install-on-linux|Install on Linux]]
- [[wiki/sources/rust-for-backend-developers-development-environment|Development Environment]]
- [[wiki/sources/rust-for-backend-developers-first-look|First Look]]
- [[wiki/sources/rust-for-backend-developers-safe-rust|Safe Rust]]

Ingested Rust for Backend Developers materials (`2. base` so far):

- [[wiki/sources/rust-for-backend-developers-variables|Variables]]
- [[wiki/sources/rust-for-backend-developers-primitive-types|Primitive Types]]
- [[wiki/sources/rust-for-backend-developers-console-output|Console Output]]
- [[wiki/sources/rust-for-backend-developers-scopes|Scopes]]
- [[wiki/sources/rust-for-backend-developers-references|References]]
- [[wiki/sources/rust-for-backend-developers-arrays|Arrays]]
- [[wiki/sources/rust-for-backend-developers-vector|Vector]]
- [[wiki/sources/rust-for-backend-developers-slices|Slices]]
- [[wiki/sources/rust-for-backend-developers-strings|Strings]]
- [[wiki/sources/rust-for-backend-developers-if|If]]
- [[wiki/sources/rust-for-backend-developers-loops|Loops]]
- [[wiki/sources/rust-for-backend-developers-functions|Functions]]
- [[wiki/sources/rust-for-backend-developers-tuples|Tuples]]
- [[wiki/sources/rust-for-backend-developers-ownership|Ownership]]
- [[wiki/sources/rust-for-backend-developers-lifetimes|Lifetimes]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros|Declarative Macros]]
- [[wiki/sources/rust-for-backend-developers-pointers|Pointers]]
- [[wiki/sources/rust-for-backend-developers-structs|Structs]]
- [[wiki/sources/rust-for-backend-developers-modules|Modules]]
- [[wiki/sources/rust-for-backend-developers-traits|Traits]]
- [[wiki/sources/rust-for-backend-developers-auto-derive-traits|Auto-Derive Traits]]
- [[wiki/sources/rust-for-backend-developers-destructuring|Destructuring]]
- [[wiki/sources/rust-for-backend-developers-pattern-matching|Pattern Matching]]
- [[wiki/sources/rust-for-backend-developers-anonymous-functions|Anonymous Functions]]
- [[wiki/sources/rust-for-backend-developers-generics|Generics]]
- [[wiki/sources/rust-for-backend-developers-enums|Enums]]
- [[wiki/sources/rust-for-backend-developers-option|Option]]
- [[wiki/sources/rust-for-backend-developers-result|Result]]
- [[wiki/sources/rust-for-backend-developers-iterators|Iterators]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers|Smart Pointers]]
- [[wiki/sources/rust-for-backend-developers-cargo|Cargo]]
- [[wiki/sources/rust-for-backend-developers-dependencies|Dependencies]]
- [[wiki/sources/rust-for-backend-developers-creating-library|Creating Library]]
- [[wiki/sources/rust-for-backend-developers-multiple-binaries|Multiple Binaries]]
- [[wiki/sources/rust-for-backend-developers-package-crate-module|Package, Crate, Module]]
- [[wiki/sources/rust-for-backend-developers-workspace-project|Workspace Project]]
- [[wiki/sources/rust-for-backend-developers-testing|Testing]]
- [[wiki/sources/rust-for-backend-developers-common-traits|Common Traits]]

Ingested Rust for Backend Developers materials (`3. advance` so far):

- [[wiki/sources/rust-for-backend-developers-common-traits|Common Traits]]
- [[wiki/sources/rust-for-backend-developers-any|Any]]
- [[wiki/sources/rust-for-backend-developers-collections|Collections]]
- [[wiki/sources/rust-for-backend-developers-io|I/O]]
- [[wiki/sources/rust-for-backend-developers-file-system|File System]]
- [[wiki/sources/rust-for-backend-developers-newtype-pattern|Newtype Pattern]]
- [[wiki/sources/rust-for-backend-developers-panic|panic]]
- [[wiki/sources/rust-for-backend-developers-multithreading|Multithreading]]
- [[wiki/sources/rust-for-backend-developers-global-data|Global Data]]
- [[wiki/sources/rust-for-backend-developers-error-handling|Error Handling]]
- [[wiki/sources/rust-for-backend-developers-serialization|Serialization]]
- [[wiki/sources/rust-for-backend-developers-date-and-time|Date and Time]]
- [[wiki/sources/rust-for-backend-developers-logging|Logging]]
- [[wiki/sources/rust-for-backend-developers-application-configuration|Application Configuration]]

Ingested Rust for Backend Developers materials (`4. async` so far):

- [[wiki/sources/rust-for-backend-developers-async-in-rust|Async in Rust]]

`3. advance` sectioni endi quyidagi yo'nalishlarni qamrab oladi:

- runtime type inspection: `Any`, `TypeId`, safe downcast
- collection selection: `Vec` defaulti, `VecDeque`, `HashSet`, `BTreeMap`, `BinaryHeap`
- byte I/O: `Read`, `Write`, `Cursor`
- filesystem boundary: `std::fs`, `Path`, `OsStr`, `OsString`, `OpenOptions`
- newtype workaround: orphan rule, local compare/conversion semantics
- panic policy: `panic!` vs `Result`, `catch_unwind`, `panic_any`, hook va placeholder macro'lar
- multithreading: threads, `Send`/`Sync`, sync primitives, scoped threads, atomics, TLS, channels
- global data: `const`, `static`, `static mut`, `LazyLock`, `OnceLock`, synchronized registry patterns
- error handling: `thiserror`, wrapping, `Box<dyn Error>`, `anyhow`, context, root cause, backtrace
- serialization: `serde`, `Serialize`, `Deserialize`, JSON/XML format crates, enum tagging, field rename, `DeserializeOwned`
- date/time: `Duration`, `SystemTime`, `Instant`, `chrono`, `Naive*`, `DateTime<Tz>`, timezone, RFC3339
- logging: `tracing`, subscriber, `EnvFilter`, `RUST_LOG`, file appender, non-blocking writer, layers, spans, `instrument`
- configuration: `config` crate, TOML config, profile layers, environment overrides, typed serde config structs

Current source baseline:

- Rust Book material assumes Rust `1.90.0` or later.
- Project examples should be read with `edition = "2024"` in `Cargo.toml`.
- Official Rust Book can be used online or offline through `rustup doc --book`.
- Rust for Backend Developers material targets experienced back-end engineers and assumes prior knowledge of HTTP, databases, JSON, console work, stack/heap, and multithreading.

Current learning frame:

- Rust aims to combine high-level ergonomics with low-level control.
- Rust's compiler is part of the learning and development workflow, not just a final checker.
- The book mixes concept chapters with project chapters; Chapter 2, Chapter 12, and Chapter 21 are the main project chapters.
- Compiler error messages should be collected into `wiki/errors/` once concrete examples appear.
- Backend-focused material should avoid repeating Rust Book basics unless it adds platform setup, tooling, or server-side context.

Chapter 1 synthesis:

- [[wiki/chapters/1-getting-started|1. Getting Started]] establishes the local workflow: install with [[rustup]], verify with [[rustc]], write [[hello-world]], then switch to [[cargo|Cargo]].
- Direct `rustc` is useful for seeing compile/run mechanics, but Cargo is the normal Rust project workflow.
- `cargo check` is the fast feedback command; `cargo build --release` is for optimized release binaries and benchmarks.

Chapter 2 synthesis:

- [[wiki/chapters/2-programming-a-guessing-game|2. Programming a Guessing Game]] introduces Rust concepts through a project rather than a reference-style explanation.
- The project practices [[variables-and-mutability|variables and mutability]], [[result|Result]], [[match]], [[loop]], [[shadowing]], [[crate]], and [[rand]].
- The first saved compiler diagnostic is [[e0308-mismatched-types|E0308 mismatched types]], caused by comparing `String` input with a numeric secret value.
- Chapter 3 now explains those concepts more systematically.

Chapter 3 synthesis:

- [[wiki/chapters/3-common-programming-concepts|3. Common Programming Concepts]] turns Chapter 2's hands-on concepts into a reference foundation.
- Rust defaults to immutable variables, supports explicit `mut`, and uses [[shadowing]] for transformations that may change type.
- [[data-types|Data types]] are known at compile time; ambiguity such as `"42".parse()` needs explicit type annotations.
- Rust functions are expression-oriented: final expressions return values, while semicolons turn expressions into statements.
- [[control-flow|Control flow]] requires explicit Boolean conditions and favors `for` loops for safe collection iteration.
- New saved diagnostics: [[e0284-type-annotations-needed|E0284 type annotations needed]] and [[e0384-cannot-assign-twice|E0384 cannot assign twice]].

Chapter 4 synthesis:

- [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]] starts Rust's unique memory model.
- [[ownership]] gives Rust memory safety without garbage collection by enforcing owner/scope/drop rules at compile time.
- [[string-type|String]] is the key example because it has stack metadata and heap data; assignment moves ownership instead of deep-copying heap data.
- [[copy-trait|Copy trait]] explains why simple stack-only values like integers remain valid after assignment.
- [[reference]] va [[borrowing]] ownershipni transfer qilmasdan value'dan foydalanish imkonini beradi.
- Borrowing qoidasi: bir vaqtda yoki bitta [[mutable-reference|mutable reference]], yoki istalgancha immutable references bo'lishi mumkin.
- [[dangling-reference|Dangling references]] compiler tomonidan rad qilinadi; references har doim valid bo'lishi kerak.
- [[slices]] collection ichidagi contiguous qismga ownershipsiz reference beradi; [[string-slice|String slice]] type'i `&str`.
- `&str` parameter `&String`dan flexibleroq bo'lgani uchun [[api-design|API design]]da yaxshi default hisoblanadi.
- New saved diagnostics: [[e0382-borrow-of-moved-value|E0382 borrow of moved value]], [[e0596-cannot-borrow-as-mutable|E0596 cannot borrow as mutable]], [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]], [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]], and [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]].

Chapter 5 synthesis:

- [[wiki/chapters/5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]] Rustda related data'ni named custom types bilan model qilishni boshlaydi.
- [[structs]] [[tuple]]dan farqli ravishda fieldsni nomlaydi, shu sababli meaning orderga kamroq bog'lanadi.
- [[struct-instances|Struct instances]] concrete valuesni saqlaydi; field access dot notation orqali bo'ladi.
- Mutation butun instance darajasida beriladi, ayrim fields alohida `mut` qilinmaydi.
- [[field-init-shorthand]] va [[struct-update-syntax]] struct literal boilerplateni kamaytiradi.
- Struct update syntax [[ownership]] qoidalariga bo'ysunadi: [[string-type|String]] field move bo'lishi, [[copy-trait|Copy trait]] fields esa copy bo'lishi mumkin.
- [[tuple-structs]] named distinct type beradi; [[unit-like-structs]] datasiz type sifatida keyin [[traits]] bilan foydali bo'lishi mumkin.
- Struct ichida `&str` reference fields saqlash [[lifetimes]] talab qiladi; beginner pattern sifatida owned [[string-type|String]] fields ishlatish soddaroq.
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]] endi dangling return va structs-with-references kontekstlarida bog'langan.
- [[rectangle-example|Rectangle area example]] separate variablesdan [[tuple]]ga, keyin named [[structs|struct]]ga refactor qilish orqali data modeling clarityni ko'rsatadi.
- Custom structs developer debugging uchun [[debug-trait|Debug trait]]ni `#[derive(Debug)]` orqali opt in qiladi; `{:?}` va `{:#?}` [[debug-formatting|Debug formatting]] beradi.
- `println!("{rect1}")` yoki `println!("{rect1:?}")` kerakli formatting trait bo'lmasa [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]ga olib keladi.
- [[dbg-macro|dbg! macro]] quick debugging uchun useful, lekin ownershipni olib qaytargani sababli kerak joyda `dbg!(&value)` ishlatiladi.
- [[methods]] behaviorni [[impl-block|impl block]] orqali struct type bilan bog'laydi; `&self` `self: &Self` shorthand.
- [[associated-functions]] `self` olmasa method emas; `Rectangle::square(3)` kabi constructor-like helpers type namespace orqali chaqiriladi.
- Rust method receivers uchun [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]] ishlatadi, shuning uchun alohida `->` operator kerak emas.

Chapter 6 synthesis:

- [[wiki/chapters/6-enums-and-pattern-matching|6. Enums and Pattern Matching]] Rustning value "qaysidir bittasi" tabiatiga ega bo'lgan domenlarni modellash imkoniyatini ochadi.
- [[enums]] mumkin bo'lgan variantlarni sanab type yaratadi; struct related fieldsni group qilsa, enum value qaysi shaklida ekanini bildiradi.
- [[enum-variants|Variantlar]] enum identifier ostida namespacelangan; har biri datasiz, tuple-like, yoki struct-like shaklda bo'lishi mumkin.
- Variant ichida data tutish struct + enum patternidan ko'pincha qisqaroq, shu bilan birga variantlar har xil shape va data miqdorida bo'lishi mumkin.
- Variant nomi avtomatik [[constructor-functions|constructor function]] bo'ladi: `IpAddr::V4` — `String`dan `IpAddr` yasaydi.
- Standard library `IpAddr` variant ichida struct ishlatadi (`V4(Ipv4Addr)`, `V6(Ipv6Addr)`), bu enum variantlari istalgan turdagi data tashishi mumkinligini ko'rsatadi.
- `Message` enum to'rt variant shaklini birlashtiradi; `impl` orqali enum'ga [[methods|method]] qo'shish struct'dagidek ishlaydi.
- [[option|Option<T>]] standard library enumi `null`'ni almashtiradi; prelude'da, `Some` va `None`'ni prefix'siz ishlatish mumkin.
- Tony Hoare null'ni "[[null-billion-dollar-mistake|billion dollar mistake]]" deb atagan; Rust uni type system orqali olib tashlab, opt-in absence semantikani [[option|Option<T>]] orqali beradi.
- [[generics]] Chapter 10'gacha `<T>` parameter shaklida tanitiladi: `Some` istalgan turdagi bitta data tutadi, har bir concrete `T` `Option<T>`ni boshqa type qiladi.
- Type-safety tutib qoladigan klassik xato — `i8 + Option<i8>` — [[e0277-trait-bound-not-satisfied|E0277]]ning yangi konteksti bo'lib qo'shildi.
- [[match]] enum va `Option<T>` variantsini [[pattern-matching|patterns]] bilan handle qiladi; arm'lar expression bo'lib, mos arm value'si butun `match` value'siga aylanadi.
- Pattern binding enum payloadini ochadi: `Coin::Quarter(state)` ichki `UsState`ni `state`ga bog'laydi.
- [[exhaustive-matching|Exhaustive matching]] `None` kabi unutilgan casesni compile time'da ushlaydi; bu yangi [[e0004-non-exhaustive-patterns|E0004 non-exhaustive patterns]] diagnostic sahifasiga qo'shildi.
- [[catch-all-patterns|Catch-all patterns]] default behavior beradi: `other` value'ni bind qiladi, `_` value'ni ignore qiladi, `_ => ()` esa explicit no-op.
- [[if-let|if let]] bitta interesting pattern uchun `match`ning concise alternativasi; kamroq boilerplate beradi, lekin exhaustive checkingdan voz kechadi.
- [[let-else|let...else]] pattern mos kelmasa early return qilib, mos kelsa bindingni outer scope'da beradi; happy pathni asosiy flowda ushlab turadi.
- [[coin-match|Coin match example]] Chapter 6.2 uchun enum variant payloadini `match`da bind qilishni alohida misol sifatida saqlaydi.

Chapter 7 synthesis:

- [[wiki/chapters/7-packages-crates-and-modules|7. Packages, Crates, and Modules]] katta Rust projectlarni tartiblash muammosini ochadi: code bitta file va bitta [[module]]dan bir nechta modules, files, [[crate|crates]], va [[package|packages]]ga o'sadi.
- [[module-system|Module system]] to'rtta asosiy feature'ni bog'laydi: [[package|packages]], [[crate|crates]], [[module|modules]] + [[use-declarations|use]], va [[paths|paths]].
- [[package]] Cargo boshqaradigan bundle; u crate'larni build, test, va share qilishga yordam beradi.
- [[crate]] compiler bir vaqtda ko'radigan eng kichik compile unit; u [[binary-crate|binary crate]] yoki [[library-crate|library crate]] bo'lishi mumkin.
- [[binary-crate|Binary crate]] executable beradi va `main` function talab qiladi; [[library-crate|library crate]] reusable functionality beradi va executable chiqarmaydi.
- [[crate-root|Crate root]] compiler boshlaydigan source file; Cargo convention bo'yicha `src/main.rs` binary crate root, `src/lib.rs` library crate root.
- Bitta package xohlagancha binary crate tutishi mumkin, lekin eng ko'pi bilan bitta library crate tutadi; `src/bin/*.rs` file'lari additional binary crates yaratadi.
- 7.2 [[module]] cheat sheet beradi: compiler crate rootdan boshlaydi, `mod` declaration orqali module code'ini inline body, `src/name.rs`, yoki `src/name/mod.rs` kabi joylardan qidiradi.
- [[module-tree|Module tree]] implicit `crate` rootidan boshlanadi; modules parent, child, va sibling munosabatlari bilan nested bo'ladi.
- [[privacy]] private by default; [[pub-keyword|pub]] module yoki item'ni public qiladi, lekin public module ichidagi item'lar avtomatik public bo'lmaydi.
- [[use-declarations|use]] long [[paths|path]]ni current [[scope]]da shortcut sifatida ishlatishga beradi, masalan `crate::garden::vegetables::Asparagus` -> `Asparagus`.
- [[backyard-module-layout|Backyard module layout]] 7.2 uchun `src/main.rs`, `src/garden.rs`, va `src/garden/vegetables.rs` file'lari module tree'ga qanday ulanayotganini ko'rsatadigan saqlangan example.
- 7.3 [[paths|path]]ni ikki shaklga ajratadi: [[absolute-path|absolute path]] `crate` yoki external crate nomidan boshlanadi, [[relative-path|relative path]] current module'dan boshlanadi.
- [[super-keyword|super]] relative path'ni parent module'dan boshlaydi; bu related parent/child code birga ko'chishi mumkin bo'lganda foydali.
- Correct path privacy'ni chetlab o'tmaydi: private `hosting` module yoki private `add_to_waitlist` function chaqirilsa [[e0603-private-item|E0603 private item]] chiqadi.
- [[public-api|Public API]] crate users bilan contract; binary + library package patternida `src/main.rs` binary crate `src/lib.rs` library crate public API'sini client kabi ishlatadi.
- `pub struct` fieldsni avtomatik public qilmaydi, lekin `pub enum` variantsni public qiladi; [[restaurant-paths-and-privacy|restaurant paths and privacy]] bu farqni saqlangan example sifatida ko'rsatadi.
- 7.4 [[use-declarations|use]]ni scope-local shortcut sifatida chuqurlashtirdi: import yozilgan [[scope]]da amal qiladi, privacy'ni chetlab o'tmaydi, va module tree'ni o'zgartirmaydi.
- Rust idiomi functionlar uchun parent module'ni import qilishni, structs/enums uchun esa itemning o'zini full path bilan olib kirishni afzal ko'radi.
- [[use-aliases|Use aliases]] `as` orqali nom collisionni hal qiladi; [[nested-use-paths|nested use paths]] bir xil prefixli importsni ixchamlaydi; [[glob-operator|glob operator]] hamma public item'larni olib kiradi, lekin readability va collision riskini oshiradi.
- [[pub-use|pub use]] re-export qilib, internal module organizationdan farqli external [[public-api|public API]] beradi; [[restaurant-reexports-and-file-layout|restaurant re-exports and file layout]] buni saqlangan example sifatida ko'rsatadi.
- External crate ishlatish uchun dependency `Cargo.toml`ga qo'shiladi va code ichida crate nomidan boshlanadigan path yoki `use` ishlatiladi; [[standard-library|standard library]] `std` crate sifatida pathda ishlatiladi, lekin `Cargo.toml`ga qo'shilmaydi.
- 7.5 [[module-file-layout|module file layout]]ni yakunladi: [[mod-declarations|mod declarations]] module file'larini crate'ga ulaydi, `use` esa file compilationga ta'sir qilmaydi.
- Modern layout `src/front_of_house.rs` va `src/front_of_house/hosting.rs`; older `mod.rs` style supported, lekin bir module uchun ikkala style birga ishlatilsa compiler error bo'ladi.
- Chapter 7 package/crate layeri, module tree/privacy layeri, path/use layeri, public API/re-export layeri, va module file layoutni birlashtirib yakunlandi.

Chapter 8 synthesis:

- [[wiki/chapters/8-common-collections|8. Common Collections]] standard librarydagi runtime grow/shrink qiladigan heap-backed [[collections|collections]] mavzusini ochadi.
- Chapter 8 uchta common collectionni ko'rsatadi: [[vector|Vec<T>]], [[string-type|String]], va [[hash-map|HashMap]].
- [[vector|Vec<T>]] bir xil concrete type elementlarini contiguous memoryda saqlaydigan growable list.
- Empty `Vec::new()` type context bo'lmasa annotation talab qiladi; [[vec-macro|vec! macro]] initial values orqali type inferencega yordam beradi.
- Vector elementiga `&v[index]` bilan access qilish out-of-bounds bo'lsa [[panic|panic]] qiladi; `v.get(index)` esa `Option<&T>` qaytaradi.
- Vector `push` capacity yetmasa reallocation qilishi mumkin; elementga active immutable reference bor paytda `push` qilish [[e0502-mutable-and-immutable-borrow-conflict|E0502]] bilan rad etiladi.
- Vector iteration `for i in &v` yoki `for i in &mut v` orqali qilinadi; mutable elementni o'zgartirish uchun [[dereference-operator|dereference operator]] `*` kerak.
- Bitta vector ichida turli data shape'larini saqlash uchun [[enums|enum]] variants payloadlaridan foydalaniladi; [[spreadsheet-cell-vector|spreadsheet cell vector]] bu patternni ko'rsatadi.
- Vector scope'dan chiqqanda vector va uning elementlari [[drop|dropped]] bo'ladi.
- 8.2 [[string-type|String]]ni growable, mutable, owned, [[utf-8|UTF-8]] encoded collection sifatida chuqurlashtirdi; `&str` borrowed string slice sifatida ajratildi.
- `String` `Vec<u8>` ustidagi wrapper bo'lib, valid UTF-8 guarantees va text methods beradi.
- String yaratish: `String::new`, `String::from`, va `to_string`; update qilish: `push_str` string slice qo'shadi, `push` bitta `char` qo'shadi.
- [[string-concatenation|String concatenation]]da `+` left `String`ni move qiladi; [[format-macro|format! macro]] formatted `String` qaytaradi va ownershipni olmaydi.
- Rust [[string-indexing|string indexing]]ni integer bilan rad qiladi, chunki bytes, Unicode scalar values (`char`), va [[grapheme-clusters|grapheme clusters]] bir xil emas.
- String range slicing byte boundaries bilan ishlaydi; UTF-8 character boundary buzilsa [[panic|panic]] bo'ladi.
- `.chars()` scalar values bo'ylab, `.bytes()` raw bytes bo'ylab yuradi; grapheme cluster iteration standard libraryda yo'q.
- 8.3 [[hash-map|HashMap]]ni key-value collection sifatida chuqurlashtirdi: lookup vector index bilan emas, meaningful key bilan qilinadi.
- `HashMap` prelude'da emas; odatda `use std::collections::HashMap;` kerak.
- `HashMap::get` `Option<&V>` qaytaradi; missing key `None`, common pattern esa `.copied().unwrap_or(default)`.
- Owned `String` key/value `insert` orqali mapga move bo'ladi, `i32` kabi [[copy-trait|Copy trait]] values copied bo'ladi.
- Existing key bilan `insert` eski value'ni overwrite qiladi; [[entry-api|entry API]] va `or_insert` esa insert-if-missing hamda old value update patternini beradi.
- [[word-count-hashmap|Word count hashmap]] `split_whitespace`, `entry`, `or_insert`, mutable reference, va `*count += 1`ni birlashtiradi.
- Default [[hashing-function|hashing function]] SipHash securityga e'tibor beradi; performance-sensitive code custom hasherni tanlashi mumkin.
- Chapter 8 complete: keyingi katta mavzu [[error-handling|error handling]] bo'lib, Chapter 9 boshlandi.

Chapter 9 synthesis:

- [[wiki/chapters/9-error-handling|9. Error Handling]] Rustda xatolarni explicit design decision sifatida ko'rsatadi: [[recoverable-errors|recoverable errors]] uchun [[result|Result<T, E>]], [[unrecoverable-errors|unrecoverable errors]] uchun [[panic|panic!]].
- Rust exceptions ishlatmaydi; error ehtimoli function return type, `match`, yoki explicit panic orqali ko'rinadi.
- Recoverable error - program userga xabar berib, retry qilib, yoki callerga error qaytarib davom etishi mumkin bo'lgan holat.
- Unrecoverable error odatda bug yoki impossible state belgisi; bunday pathda programni davom ettirish to'g'ri emas.
- 9.1 [[panic|panic!]] macro'ni chuqurlashtirdi: panic bevosita macro chaqiruvidan yoki invalid operationdan, masalan out-of-bounds [[vector-indexing|vector indexing]]dan kelishi mumkin.
- Default panic [[stack-unwinding|stack unwinding]] qiladi; [[abort-on-panic|abort on panic]] uchun [[cargo-toml|Cargo.toml]] release profile'da `panic = 'abort'` yoziladi.
- `v[99]` kabi invalid index Rustda undefined behavior emas; panic orqali [[buffer-overread|buffer overread]] va memory disclosure risklari to'xtatiladi.
- `RUST_BACKTRACE=1 cargo run` [[backtrace]] beradi; uni o'qishda birinchi o'zingiz yozgan source file line'ini topish kerak.
- User-provided index yoki normal input errorlari uchun `[]`/panic o'rniga `get`/`Option` yoki `Result` bilan recoverable path tanlash yaxshiroq.
- 9.2 [[result|Result<T, E>]]ni generic recoverable error type sifatida chuqurlashtirdi: `Ok(T)` success value, `Err(E)` error value.
- `File::open("hello.txt")` `Result<std::fs::File, std::io::Error>` qaytaradi; success bo'lsa [[file-handle|file handle]], failure bo'lsa [[io-error|io::Error]].
- [[error-kind|ErrorKind::NotFound]] kabi variants error reason bo'yicha recovery qilishga imkon beradi; missing file create qilinishi, boshqa error panic qilishi yoki propagate qilinishi mumkin.
- [[unwrap]] va [[expect]] `Result` yoki `Option`ni panic-on-error shortcut sifatida ochadi; `expect` message assumptionni debugging uchun aniqroq qiladi.
- [[error-propagation|Error propagation]] callerga qaror berish patterni; [[question-mark-operator|? operator]] `Ok`ni ochib, `Err`ni early return qiladi.
- `?` operator [[from-trait|From trait]] orqali error type conversion qilishi mumkin, lekin faqat compatible return type'da ishlaydi.
- `()` qaytaradigan `main` ichida `Result` ustida `?` ishlatish [[e0277-trait-bound-not-satisfied|E0277]] beradi; [[main-function|main]] `Result<(), Box<dyn Error>>` qaytarsa `?` ishlaydi.
- `?` `Option` bilan ham ishlaydi: `None` early return, `Some(value)` inner value; `Result` va `Option` automatic mix qilinmaydi.
- 9.3 [[panic-vs-result|panic! vs Result]] qarorini yakunladi: fail bo'lishi mumkin bo'lgan function uchun `Result` yaxshi default, chunki callerga recovery yoki panic tanlovini beradi.
- Examples, prototype code, va [[testing|tests]]da `unwrap`/`expect` acceptable bo'lishi mumkin; testsda panic test failure signalidir.
- Developer compilerdan ko'proq ma'lumotga ega bo'lsa, [[expect]] assumptionni message bilan hujjatlashtirishi mumkin, masalan hardcoded valid IP parse.
- [[bad-state|Bad state]] - assumption, guarantee, [[function-contracts|contract]], yoki [[invariants|invariant]] buzilishi; continuing harmful yoki insecure bo'lsa panic mos.
- Expected failures - malformed parser input, HTTP rate limit, wrong user format - `Result` bilan qaytarilishi kerak.
- Rust type system runtime checksni kamaytiradi: non-`Option` parameter value borligini, `u32` negative emasligini bildiradi.
- [[custom-validation-types|Custom validation types]] invariantni API ichida encode qiladi; [[guess-validation-type|Guess validation type]] 1..=100 range'ni constructor va private field orqali saqlaydi.
- Public API panic qilishi mumkin bo'lgan contract violations documentationda yozilishi kerak.
- Chapter 9 complete: keyingi mavzu [[generics]].

Chapter 10 synthesis:

- [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]] Rust abstraction toolsini ochadi: [[generics]], [[traits]], va [[lifetimes]].
- Generics concrete type yoki property o'rnida abstract placeholder beradi; oldingi examples: [[option|Option<T>]], [[vector|Vec<T>]], [[hash-map|HashMap<K, V>]], va [[result|Result<T, E>]].
- Chapter 10 opening o'zimiz generic functions, structs, enums, va methods define qilishga tayyorlaydi.
- [[traits]] generic type qaysi behaviorga ega bo'lishi kerakligini constraint qiladi.
- [[lifetimes]] references relationshipni compilerga aytadigan generics turi sifatida frame qilinadi.
- [[code-duplication|Code duplication]] tedious va error-prone; abstraction bitta conceptni bitta joyda ifodalashga yordam beradi.
- [[function-extraction|Function extraction]] steps: duplicate code'ni topish, inputs/outputsni signature'da ko'rsatish, duplicate sitesni function callga almashtirish.
- [[largest-function-extraction|Largest function extraction]] `largest(list: &[i32]) -> &i32` orqali borrowed slice qabul qilib, largest elementga reference qaytaradi.
- Function body concrete values o'rniga abstract parameter ustida ishlaganidek, generics concrete types o'rniga abstract type parameters ustida ishlaydi.
- 10.1 [[generic-functions|generic functions]] syntaxini ochdi: `fn largest<T>(list: &[T]) -> &T` type parameter e'lon qiladi, lekin `>` ishlatilsa `T: std::cmp::PartialOrd` trait bound talab qilinadi.
- [[e0369-binary-operation-cannot-be-applied|E0369]] generic body har qanday `T` uchun ishlamasligini ko'rsatadi; compiler [[partial-ord|PartialOrd]] constraint taklif qiladi.
- [[generic-structs|Generic structs]] `Point<T>` orqali same-type fieldsni, `Point<T, U>` orqali different-type fieldsni model qiladi; noto'g'ri mixed fields [[e0308-mismatched-types|E0308]] beradi.
- [[generic-enums|Generic enums]] `Option<T>` va `Result<T, E>`ni reusable abstraction sifatida tushuntiradi.
- [[generic-methods|Generic methods]] `impl<T> Point<T>`, concrete-only `impl Point<f32>`, va method-level generics (`mixup<X2, Y2>`) farqini ko'rsatadi.
- [[monomorphization]] Rust generics performance modelidir: compiler generic code'ni concrete type versionsga aylantiradi, shuning uchun generics runtime cost keltirmaydi.
- 10.2 [[traits]]ni shared behavior contracti sifatida chuqurlashtirdi: [[trait-definitions|trait definitions]] method signaturesni belgilaydi, [[trait-implementations|trait implementations]] esa `impl Trait for Type` bilan concrete behavior beradi.
- [[summary-trait-example|Summary trait example]] `NewsArticle` va `SocialPost` turli data shape'lari bitta `summarize()` behaviorini share qilishini ko'rsatadi.
- [[orphan-rule|Orphan rule]] coherence saqlaydi: trait yoki type'dan kamida bittasi local bo'lmasa implementation yozib bo'lmaydi.
- [[default-trait-implementations|Default trait implementations]] trait ichida common body berib, implementor uni keep yoki override qilishiga imkon beradi.
- [[impl-trait|impl Trait]] simple parameter va return-position abstraction beradi; return position bitta concrete type qaytarishi kerak.
- [[trait-bounds|Trait bounds]] va [[where-clauses|where clauses]] generic type capabilitiesni aniq cheklaydi: `T: Summary`, `T: Summary + Display`, yoki `where T: Display + Clone`.
- [[conditional-method-implementations|Conditional method implementations]] `Pair<T>` kabi generic type'larda methodni faqat `T: Display + PartialOrd` bo'lganda beradi.
- [[blanket-implementations|Blanket implementations]] traitni boundga mos barcha typelar uchun implement qiladi; standard library `Display` implement qilgan typelar uchun `ToString` beradi.
- 10.3 [[lifetimes]]ni to'liq ochdi: [[dangling-reference|dangling reference]], [[borrow-checker|borrow checker]], funksiya/struct/method signaturalarida lifetime annotatsiyalar, [[lifetime-elision|lifetime elision]] va [[static-lifetime|'static lifetime]].
- Lifetimes genericsning bir turi sifatida: `'a` type parameter kabi angle bracket ichida e'lon qilinadi, lekin typelar o'rniga reference munosabatlarini ifodalaydi.
- Borrow checker `'a` = kichik lifetime qoidasi bilan ishlaydi: return reference har doim parametrlarning kichik lifetimesigacha valid.
- Lifetime annotatsiyalar lifetimeni o'zgartirmaydi — ular faqat borrow checker uchun constraint ifodalaydi.
- Lifetime elision 3 qoidasi: (1) har bir parameter o'z lifetime'ini oladi; (2) bitta input lifetime bo'lsa, u output'ga beriladi; (3) method bo'lsa, `&self` lifetime output'ga beriladi.
- `'static` — butun dastur davomida yashovchi lifetime; string literallar shu lifetimega ega (binary ichida saqlanadi).
- Yangi diagnostics: [[e0597-does-not-live-long-enough|E0597 does not live long enough]], [[e0515-cannot-return-local-reference|E0515 cannot return local reference]].
- [[e0106-missing-lifetime-specifier|E0106]] endi funksiya ikkita reference parameter va return bilan ham keng tushuntirildi.
- Chapter 10 yakunlandi. Generics + Traits + Lifetimes birgalikda: `longest_with_an_announcement<'a, T>` uchta feature'ni bir funksiyada ko'rsatadi.
- Keyingi: Chapter 11 (Testing) yoki Chapter 12 (I/O Project).

Chapter 11 synthesis (qisman — 11.0 va 11.1):

- [[wiki/sources/11-writing-automated-tests|Chapter 11]] Rust testlash mexanikasini ochadi: annotatsiyalar/macrolar (11.1), test ishlatish optsiyalari (11.2), unit va integration tests tashkiloti (11.3).
- [[testing|Testing]] Rustda type system va borrow checker yetib bormagan mantiqiy xatolarni ushlaydi; Dijkstra: "testlar buglar borligini ko'rsatadi, yo'qligini isbotlamaydi."
- [[cfg-test|`#[cfg(test)]`]] faqat `cargo test`da compile bo'ladigan `mod tests` blokini belgilaydi; release binary'ga test kodi kirmaydi.
- [[test-macros|Test macro'lar]]: `assert!` boolean tekshiradi; `assert_eq!` / `assert_ne!` tenglik/tengsizlik tekshirib, xatoda ikkala qiymatni chiqaradi — `PartialEq + Debug` talab qiladi.
- Custom failure messages — `assert!`, `assert_eq!`, `assert_ne!`ga format string qo'shimcha argument sifatida beriladi.
- [[should-panic|`#[should_panic]`]] kodning panic qilishini testlaydi; `expected = "substring"` bilan aniqlashtirish mumkin.
- `Result<T, E>` returning testlar `?` operator ishlatishga imkon beradi; `#[should_panic]` bilan mos kelmaydi.
- 11.2 [[11-2-controlling-how-tests-are-run|Controlling How Tests Are Run]] test binary argumentlarini `--` bilan ajratishni o'rgatdi.
- Default parallel ishlatish shared state muammolariga olib kelishi mumkin; `--test-threads=1` ketma-ket ishlatish beradi.
- O'tgan testlar outputi default'da ushlanadi; `-- --show-output` bilan ko'rsatish mumkin.
- [[test-filtering|Test filtrlash]]: `cargo test <pattern>` — nomi substringga mos barcha testlar ishlaydi; modul nomi ham qidiriladi.
- `#[ignore]` og'ir testlarni standart rundan chiqaradi; `-- --ignored` va `-- --include-ignored` maxsus holatlarda ishlatiladi.
- 11.3 [[11-3-test-organization|Test Organization]] ikki kategoriyani rasmiylashtirdi: [[unit-tests]] va [[integration-tests]].
- [[unit-tests|Unit tests]] `src/` ichida, `#[cfg(test)] mod tests` pattern bilan; `use super::*` private funksiyalarni ham ko'radi — to'g'ridan-to'g'ri test qilish mumkin.
- [[integration-tests|Integration tests]] `tests/` papkasida; har bir fayl alohida crate, `use crate_name::fn` kerak, `#[cfg(test)]` shart emas.
- `cargo test` output uch bo'lim: unit → integration → doc-tests; birinchi bo'lim FAILED bo'lsa keyingisi ishlamaydi.
- Shared integration helper uchun `tests/common/mod.rs` pattern — `tests/common.rs` emas (u separate crate sifatida ko'rinib qoladi).
- Binary crate cheklovi: `src/main.rs` import qilinmaydi; yaxshi dizayn — logika `src/lib.rs`da, `main.rs` minimal.
- Chapter 11 to'liq yakunlandi. Keyingi: Chapter 12 (I/O Project) yoki Chapter 13 (Iterators and Closures).

Chapter 13 synthesis (13.0, 13.1, 13.2):

- [[13-functional-language-features|Chapter 13]] Rust'ning functional programming ta'sirini ochadi: [[closures]] va [[iterators]] — tez va idiomatic kod yozishning asosi.
- [[closures]] — `|params| body` shaklida anonim funksiya; o'zgaruvchida saqlanadi yoki argument sifatida uzatiladi. Oddiy funksiyadan asosiy farqi: aniqlangan scope'dagi qiymatlarni **capture** qila oladi.
- Capture uch xil bo'ladi: **immutable borrow** (faqat o'qish), **mutable borrow** (o'zgartirish), va **`move`** (ownership o'tkazish — thread'larda zarur).
- **Type inference**: closure type annotatsiyasi shart emas, lekin birinchi chaqiruvda tip qotib qoladi; boshqa tip bilan chaqirib bo'lmaydi (E0308).
- [[fn-traits|Fn traits]] — additive pog'onali hierarchy: `FnOnce` (bir marta chaqirish kafolati yetadi), `FnMut` (bir necha marta chaqirish mumkin, mutatsiya bo'lishi mumkin), `Fn` (shared borrow bilan qayta-qayta chaqirish mumkin). Barcha closures kamida `FnOnce` implement qiladi; concurrency uchun hali `Send`/`Sync`/`'static` kabi alohida boundlar kerak bo'lishi mumkin.
- `unwrap_or_else` → `FnOnce` (eng moslashuvchan); `sort_by_key` → `FnMut` (har element uchun bir marta chaqiriladi).
- [[iterators]] — **lazy** pattern: `Iterator` trait `next()` metodini talab qiladi; `for` loop ichida implicit ravishda ishlatiladi.
- Uch xil iterator: `.iter()` (`&T`), `.into_iter()` (`T` — owned), `.iter_mut()` (`&mut T`).
- [[consuming-adapters]] (`sum`, `collect`, `count`, `fold`) — iteratorni tugata ishlatadi; keyin qayta foydalanib bo'lmaydi.
- [[iterator-adapters]] (`map`, `filter`, `zip`, `enumerate`) — yangi lazy iterator qaytaradi; oxirida consuming adapter chaqirilishi kerak, aks holda warning: "iterators are lazy and do nothing unless consumed".
- Closures + iterators = kuchli chaining: `v.iter().filter(...).map(...).collect()` — C-darajasidagi tezlik bilan ([[zero-cost-abstractions]]).
- Chapter 13 qisman yakunlandi (13.0, 13.1, 13.2). Keyingi: 13.3 (minigrep'ni closures/iterators bilan yaxshilash) va 13.4 (performance comparison).
- 13.3 `Config::build` ni `&[String]` → `impl Iterator<Item = String>` ga o'zgartirdi: `env::args()` iterator to'g'ridan-to'g'ri uzatiladi, `Vec<String>` alloc va `clone()` yo'q; `args.next()` + `match Some/None` — index'dan xavfsizroq.
- 13.3 `search` funksiyasi mutable `results` Vec + for loop dan `.lines().filter(|line| line.contains(query)).collect()` ga qisqardi — [[iterator-adapters]] va [[closures]] birgalikda.
- 13.4 benchmark (Sherlock Holmes, "the" so'zi): for loop 19.6ms, iterator 19.2ms — deyarli teng. Compiler loop unrolling va bounds check elimination qo'llaydi. Bjarne Stroustrup zero-overhead qoidasi: "What you use, you couldn't hand code any better." — [[zero-cost-abstractions]] iterators/closures uchun ham isbotlangan.
- Chapter 13 to'liq yakunlandi. Keyingi: Chapter 14 (Cargo va crates.io) yoki Chapter 15 (Smart Pointers).

Chapter 14 synthesis (14.0–14.5):

- [[wiki/chapters/14-more-about-cargo-and-crates-io|Chapter 14]] Cargo'ning ilg'or imkoniyatlarini ochadi: release profillari, crates.io'ga publish qilish, workspaces, `cargo install`, va custom subcommandlar.
- 14.1: [[release-build|Release profiles]] — `dev` (`opt-level = 0`, tez compile) va `release` (`opt-level = 3`, tez runtime). `Cargo.toml`'da `[profile.*]` bo'limlari orqali override qilish mumkin.
- 14.2: [[documentation-comments|Doc comments]] — `///` keyingi item, `//!` container (crate/module) uchun; `cargo doc --open` HTML generatsiya qiladi. `# Examples` bloki `cargo test` bilan doc-test sifatida ishga tushadi — dokumentatsiya va kod sinxronligini ta'minlaydi. [[pub-use|pub use]] ichki modul tuzilmasidan mustaqil, foydalanuvchi uchun qulay public API yaratishga imkon beradi.
- 14.2: [[crates-io|crates.io]] publish workflow — `cargo login`, majburiy metadata (`description`, `license` SPDX), `cargo publish` (mangu, o'chirib bo'lmaydi), `cargo yank` (yangi loyihalarga bloklash, mavjudlarni buzmasdan).
- 14.3: [[cargo-workspaces|Cargo workspaces]] — bitta `Cargo.lock` va `target/` ostida bir nechta package; ichki dependency `path = ...` bilan explicit; tashqi dependency har crate'da alohida; `cargo run/test -p name`.
- 14.4: `cargo install` binary target bo'lgan crate'larni `~/.cargo/bin/` ga o'rnatadi; `$PATH`'da bo'lishi kerak.
- 14.5: `cargo-something` naming pattern - `cargo something` sifatida chaqirish; `cargo --list` barcha subcommandlarni ko'rsatadi.
- Chapter 14 to'liq yakunlandi. Keyingi: Chapter 15 (Smart Pointers).

Chapter 15 synthesis (15.0-15.2):

- [[wiki/chapters/15-smart-pointers|Chapter 15]] Rustdagi pointer-like abstractionlarni ochadi: [[reference|references]] data'ni borrow qiladi, [[smart-pointers|smart pointers]] esa ko'pincha data'ga ownership va qo'shimcha behavior beradi.
- 15.0: smart pointerlar odatda [[structs|struct]] sifatida implement qilinadi va [[deref-trait|Deref trait]] hamda [[drop|Drop trait]] bilan bog'liq. Bob keyin `Box<T>`, `Rc<T>`, `RefCell<T>`, interior mutability, va reference cyclesga o'tadi.
- 15.1: [[box-t|Box<T>]] value'ni heapda saqlaydi; stackda pointer metadata turadi. `Box<T>`ning boshlang'ich asosiy use case'i [[recursive-types|recursive types]] uchun indirection berish: `Cons(i32, List)` infinite size, `Cons(i32, Box<List>)` finite size.
- 15.1: yangi diagnostic [[e0072-recursive-type-has-infinite-size|E0072]] recursive enum direct self-reference qilganda chiqadi. [[box-cons-list|Box cons list]] example sahifasi saqlandi.
- 15.2: [[dereference-operator|Dereference operator]] `*` oddiy reference va `Deref` implement qilgan smart pointer uchun inner value'ga yetadi. Custom `MyBox<T>` uchun `Deref` implement qilinmasa [[e0614-type-cannot-be-dereferenced|E0614]] chiqadi.
- 15.2: [[deref-coercions|Deref coercions]] `&MyBox<String>`ni `&String`, keyin `&str`ga function call paytida avtomatik moslaydi; bu compile time'da hal bo'ladi, runtime cost keltirmaydi. [[mybox-deref|MyBox Deref example]] saqlandi.
- Chapter 15 qisman qoplandi (15.0, 15.1, 15.2). Keyingi: 15.3 (`Drop trait`) va undan keyingi smart pointer patterns.

Chapter 17 synthesis (17.0, 17.1):

- [[17-async-programming|Chapter 17]] thread'li concurrencyga muqobil yondashuv — async programmingni ochadi: [[future|Future]] trait, [[async-await|async/await]] sintaksisi, va runtime/executor roli.
- [[cpu-bound|CPU-bound]] (video render) vs [[io-bound|I/O-bound]] (tarmoq yuklab olish) farqi muhim: I/O-bound holatda CPU ko'p vaqt kutadi; async shu kutish vaqtida boshqa ishlarni bajarish imkonini beradi.
- Concurrency (navbat bilan almashinish) va parallelism (haqiqiy bir vaqtda) aniq ajratildi; async Rust odatda concurrent ishlaydi, runtime va hardware ga qarab parallelism ham mumkin.
- [[future|Future]] trait — lazy qiymat: `.await` bo'lmasa hech narsa bajarmaydi. Iterator `next()` bilan o'xshashlik: ikkalasi ham so'rab olinmasa harakatlanmaydi.
- `async fn` compilator tomonidan `fn -> impl Future<Output = T>` ga desugared bo'ladi; `await` esa polling + state machine transitioni.
- Har bir `await point` compilator yaratadigan yashirin [[async-state-machine|state machine]]ning bir holati. Ownership/borrowing bu state machine uchun ham compile time'da tekshiriladi.
- `main` funksiyasi async bo'lolmaydi — `trpl::block_on` yoki `#[tokio::main]` orqali runtime ishga tushiriladi.
- `trpl::select` ikkita future'dan birinchi tayyor bo'lganini `Either::Left / Right` sifatida qaytaradi — birinchi amaliy concurrent racing pattern.
- [[async-page-scraper|Async page scraper]] — ikki URL'dan `<title>` ni concurrent oluvchi to'liq ishlaydigan mini dastur (Listing 17-1 dan 17-5 gacha).
- [[wiki/chapters/17-2-applying-concurrency-with-async|17.2]] thread-based concurrencyning async ekvivalentlarini ko'rsatdi: `spawn_task` vs `thread::spawn`, async channel vs `std::sync::mpsc`, `join`/`join!` vs `.join()`.
- Bitta `async {}` blok ichidagi hamma kod ketma-ket bajariladi — concurrent bo'lish uchun `trpl::join` bilan alohida bloklar kerak.
- `async move` — closure'dagi `move` kabi; ownership async blokka o'tkazadi; channel sender'ni blok oxirida drop qilish uchun zarur.
- `trpl::join` — ikki future; `trpl::join!` — N ta future (compile-time soni); `join_all` — noma'lum sondagi future'lar (keyingi bo'lim).
- [[wiki/chapters/17-3-working-with-any-number-of-futures|17.3]]: `await` nuqtasi orasidagi kod sinxron — uzoq CPU ish boshqa future'larni "ochlik"ka mahkum etadi (starvation). `trpl::yield_now().await` — runtime'ga kooperativ boshqaruv qaytaradi; `sleep(1ms)`dan tez chunki timer yaratmaydi.
- 17.3: `timeout` funksiyasi — `select` + `sleep` bilan compose qilingan custom async abstraktsiya. Pattern: `Left(result)` → `Ok`, `Right(())` → `Err(duration)`.
- [[wiki/chapters/17-4-streams-futures-in-sequence|17.4]] (qisman): Stream — async iterator. `stream_from_iter`, `StreamExt::next()`, `while let Some(v) = stream.next().await`. Raw fayl to'liq emas.
- [[wiki/chapters/17-5-a-closer-look-at-the-traits-for-async|17.5]] async trait'larning ichini ochadi. `Future::poll` aslida `Poll<T>` qaytaradigan metod; `await` shu polling loopining ergonomik yozuvi. Async state machine **self-referential** bo'lishi mumkin — ko'chirilsa dangling reference. `Pin<Box<T>>` va `pin!` macro bu muammoni hal qiladi.
- 17.5: `Unpin` — marker trait: "ko'chirish xavfsiz" degani. `String`, `Vec` kabi aksariyat tiplar `Unpin`; async future'lar `!Unpin`. `join_all` bilan `Vec` ichida future'lar ishlatish uchun `pin!` macro kerak.
- 17.5: `Stream` trait — `poll_next(Pin<&mut Self>, cx) -> Poll<Option<Item>>`. Iterator + Future birikmas. `StreamExt` yuqori qatlam metodlari — `Stream` implementatsiyasi yetarli, `StreamExt` avtomatik ishlaydi.
- [[wiki/chapters/17-6-futures-tasks-and-threads|17.6]] yakunlaydi: Future (eng mayda) → Task (runtime-boshqariluvchi) → Thread (OS-boshqariluvchi). CPU-bound → thread, I/O-bound → async. Birgalikda ham ishlaydi: thread produce + async consume (Listing 17-25). Work stealing: runtime task'larni thread'lar orasida avtomatik ko'chiradi.
- Keyingi: 17.4 raw faylni to'ldirish.

Chapter 18 synthesis (18.0, 18.1, 18.2):

- [[18-object-oriented-programming|18-bob]] OOPning asosiy uch xususiyatini Rust'da qanday ifodalanishini ko'rsatdi. Rust ba'zi OOP xususiyatlarini (objects, encapsulation, polymorphism) qo'llaydi, lekin **inheritance yo'q** — bu ataylab qilingan dizayn qarori.
- [[encapsulation]]: `pub`/private orqali. `AveragedCollection` — klassik namuna: `list` va `average` private, invariant metodlar orqali himoyalangan. Implementation o'zgarganda klient kodi o'zgarmaydi.
- Inheritance'ning ikkita maqsadi Rust'da boshqacha yechiladi: **kod qayta ishlatish** → default trait metodlar; **runtime polimorfizm** → trait objects (`dyn Trait`).
- [[trait-object|Trait objects]] (`Box<dyn Draw>`) heterojen to'plam beradi: kutubxona yozilgandan keyin foydalanuvchi yangi turlar qo'sha oladi. Generics esa homojen + static dispatch.
- [[dynamic-dispatch|Dynamic dispatch]]: vtable orqali runtime'da to'g'ri metod topiladi. Statik dispatch'ga nisbatan overhead bor, lekin kengaytiriluvchilik kerak bo'lganda zarur tradeoff.
- [[gui-draw-trait|GUI namunasi]]: `Screen` + `Vec<Box<dyn Draw>>` + `Button`/`SelectBox` — trait objects'ning kanonik ko'rsatmasi.
- [[wiki/chapters/18-3-implementing-an-object-oriented-design-pattern|18.3]] ikki usulni solishtirdi: OOP state pattern (`Box<dyn State>`) va Rust-idiomatic typestate pattern (holat → tur). OOP'da invalide holat runtime'da tutiladi; typestate'da compile-time'da taqiqlanadi (`DraftPost::content()` metodi yo'q).
- `self: Box<Self>` receiver — faqat `Box` ichida chaqiriladi, ownership oladi. `Option::take()` triki — `&mut self` orqali field'dan ownership olish.
- Typestate qoidasi: `DraftPost::request_review(self) -> PendingReviewPost` — eski tur consumed, caller `let post = post.x()` shadowing bilan davom etadi.
- Keyingi: Chapter 19 (Pattern Matching).

Chapter 19 synthesis (19.0, 19.1, 19.2):

- [[wiki/chapters/19-patterns-and-matching|Chapter 19]] [[pattern-matching|pattern matching]]ning to'liq manzarasini ochadi: kirish (19.0), barcha joylari (19.1), va refutability (19.2).
- 19.0: [[pattern-matching|Pattern]] — type strukturasiga mos keluvchi maxsus sintaksis. Komponentlari: literals, destructured arrays/enums/structs/tuples, variables, wildcards (`_`), placeholders (`..`).
- 19.1: Patternlar 6 ta kontekstda ishlatiladi: `match` armlar, `let` iboralari, `if let`, `while let`, `for` looplari, funksiya parametrlari. Ko'pincha bilmasdan ishlatiladi (`let x = 5;`da ham pattern bor).
- [[pattern-destructuring|Pattern destructuring]] `let (x, y, z) = (1, 2, 3);`, `for (i, v) in v.iter().enumerate()`, `fn f(&(x, y): &(i32, i32))` misollari orqali murakkab typelarni ichki qismlarga ajratadi.
- [[while-let|while let]] loop — pattern mos kelgunicha davom etadi; channel'dan `Ok(v)` kelguncha o'qish uchun ideal. `Err` qaytganda loop to'xtaydi.
- `if let` bilan `else if`, `else if let` zanjirini aralashtirib ishlatish mumkin; `match`dan farqi — exhaustiveness tekshirilmaydi.
- Shadowing eslatma: `if let Ok(age) = age` yangi `age` yaratadi, eski `age` shadow bo'ladi; `if let Ok(age) = age && age > 30` yozib bo'lmaydi.
- 19.2: [[irrefutable-pattern|Irrefutable pattern]] — har qanday qiymatga mos keladi (`x`, `(a, b)`). [[refutable-pattern|Refutable pattern]] — ba'zi qiymatlar uchun mos kelmaydi (`Some(x)`, `Ok(v)`).
- Qoida: `let`, `fn` params, `for` → faqat irrefutable; `if let`, `while let`, `let...else` → refutable ham qabul qiladi (irrefutable bo'lsa warning).
- `let Some(x) = value;` — `E0005: refutable pattern in local binding`; yechim: `let Some(x) = value else { return; };`.
Chapter 19.3 synthesis:

- [[wiki/chapters/19-3-pattern-syntax|19.3]] barcha pattern sintaksis variantlarini to'plagan reference bo'lim.
- **Literal pattern** — aniq qiymat bilan mos kelish: `1 => ...`, `'a' => ...`.
- **Named variable shadowing** — `match`/`if let` ichidagi pattern nom tashqi o'zgaruvchini shadow qiladi. Outer qiymat bilan solishtirish uchun [[match-guard|match guard]] kerak: `Some(n) if n == y`.
- **`|` or-pattern** — bir arm'da bir nechta pattern: `1 | 2 => ...`. Match guard bilan: `4 | 5 | 6 if cond` — `if` butun guruhga tegishli.
- **`..=` range pattern** — faqat numeric va `char`: `1..=5`, `'a'..='j'`.
- **Struct destructuring** — shorthand `Point { x, y }`, rename `Point { x: a, y: b }`, literal field `Point { x, y: 0 }`.
- **Enum destructuring** — variant shakliga qarab: no-data, tuple-like, struct-like. Nested enum/struct ham mumkin.
- **`_` va `_name` farqi** — `_` bind qilmaydi (ownership saqlanadi); `_name` bind qiladi va non-Copy typelarni move qiladi.
- **`..` placeholder** — struct'ning qolgan fieldlarini yoki tuple o'rtasini ignore qiladi. Ikki marta ishlatish noaniqlik — compile xatosi.
- **[[match-guard|Match guard]]** — pattern'dan keyin `if` sharti; faqat `match`da; exhaustiveness checker qiyinlashadi.
- **[[at-binding|@ binding]]** — `id @ 3..=7`: test + bind bitta yozuvda.
- Chapter 19 to'liq yakunlandi (19.0–19.3).

Chapter 20 + 20.1 synthesis (Advanced Features kirish + Unsafe Rust):

- [[wiki/chapters/20-advanced-features|Chapter 20]] kirish — 5 ta advanced mavzu sanab o'tiladi: unsafe, advanced traits, advanced types, advanced functions/closures, macros. Bu xususiyatlar har kuni emas, kerak bo'lganda murojaat qilinadigan reference.
- [[wiki/chapters/20-1-unsafe-rust|20.1]] [[unsafe-rust|Unsafe Rust]] ni batafsil ochadi. Asosiy xulosa: `unsafe` "tekshiruvlarni o'chirmaydi" — borrow checker, type checker, lifetime'lar — barchasi davom etadi. `unsafe` faqat **5 ta superpower** ga ruxsat beradi.
- [[unsafe-superpowers|5 ta superpower]]: (1) [[raw-pointer|raw pointer]] dereference, (2) unsafe function chaqirish, (3) [[mutable-static|mutable static]]ga kirish, (4) [[unsafe-trait|unsafe trait]] implement, (5) [[union-type|union]] field'iga kirish.
- **Yangi sintaksis (Rust 2024):** [[raw-borrow-operators|raw borrow operators]] `&raw const x`, `&raw mut x` — eski `&x as *const _` cast'idan xavfsizroq, oraliq reference yaratmaydi. `unsafe extern "C"` blokda `safe fn` keyword — xavfsiz tashqi funksiyalarni alohida belgilash. `#[unsafe(no_mangle)]` — Rust funksiyasini boshqa tilga ochish.
- **Eng muhim pattern:** [[safe-abstraction|Safe abstraction over unsafe]] — ichkarida `unsafe`, tashqarida safe API. Klassik misol: `split_at_mut` ([[split-at-mut-unsafe]]). Borrow checker bir slice'ni ikki marta mutably borrow qila olmaydi (E0499), lekin invariant (`mid <= len`) saqlangani uchun safe abstraction. Standart kutubxona deyarli butunlay shu pattern ustida qurilgan.
- **[[ffi|FFI]]** (`extern "C"` orqali) — C kutubxonalari bilan ishlash. `unsafe extern "C"` blokdagi har item implicitly unsafe; `safe fn` istisnosi mumkin. Rust 2024'da `#[unsafe(no_mangle)]` attribute nomli to'qnashuv xavfini ifodalaydi.
- **[[mutable-static|Mutable static]]** — global mutable. O'qish va yozish unsafe. `&COUNTER` reference — `static_mut_refs` lint orqali deny; raw borrow operator (`*(&raw const COUNTER)`) ishlatish kerak. Multi-thread'da deyarli har doim noto'g'ri tanlov; `Atomic*`, `Mutex<T>`, `OnceLock` ishlatish kerak.
- **[[unsafe-trait|Unsafe trait]]** — `Send` va `Sync` ning kelib chiqishi. Auto-derived; raw pointer'li struct uchun `unsafe impl Send` qo'lda yozish mumkin (dasturchi va'da beradi).
- **[[union-type|Union]]** — C interop uchun. Sof Rust'da `enum` (tagged union) idiomatic alternativ. `Drop` impl'li turlar uchun `ManuallyDrop<T>` kerak.
- **[[miri|Miri]]** — runtime [[undefined-behavior|UB detector]]. Static borrow checker'ni to'ldiradi. Nightly toolchain. `cargo +nightly miri test` — testlar Miri ostida UB'ni ushlash uchun.
- **`SAFETY:` izohi konventsiyasi** — har `unsafe fn` tepasida va har `unsafe` blok yonida; saqlash kerak bo'lgan invariantlarni hujjatlash.

Chapter 20.2 synthesis (Advanced Traits):

- [[wiki/chapters/20-2-advanced-traits|20.2]] trait'larni chuqurroq ochadi: 5 ta asosiy mavzu — [[associated-types|associated types]], [[default-generic-parameters|default generic parameters]], method [[fully-qualified-syntax|disambiguation]], [[supertraits]], va [[newtype-pattern]].
- **[[associated-types|Associated type]]** — `type Item;` placeholder. Generic'dan farq qiladi: associated bilan **bitta tip uchun bitta implementatsiya** mumkin → caller annotatsiya bermaydi (`counter.next()` avtomatik `Option<u32>`). `Iterator::Item` klassik misol. API kontrakt qismi.
- **[[default-generic-parameters|Default generic parameter]]** — `<Rhs = Self>` sintaksisi. `Add` traitining haqiqiy ta'rifi: `trait Add<Rhs = Self> { type Output; fn add(self, rhs: Rhs) -> Self::Output; }`. Default ikki maqsadda: (1) backward-compatible kengaytirish, (2) maxsus holatlarda customization (`Millimeters + Meters` — Rhs = Meters).
- **[[operator-overloading|Operator overloading]]** — `std::ops` ichidagi trait'larni implement qilish. Rust faqat **mavjud** operatorlarni overload qilishga ruxsat beradi (`+` → `Add`, `-` → `Sub`, va h.k.). Yangi operator yaratib bo'lmaydi.
- **Method disambiguation** — bir tip ko'p trait implement qilsa bir xil nomli metod bilan: `person.fly()` default'da type'ga to'g'ridan-to'g'ri impl'ga boradi. `Pilot::fly(&person)` — `&person` orqali aniq trait implementatsiyasi tanlanadi. Method'lar uchun (`&self` bilan) shu yetarli.
- **[[fully-qualified-syntax|Fully qualified syntax]]** — `<Type as Trait>::function(args)` — `self`'siz associated function'lar uchun zarur. Misol: `<Dog as Animal>::baby_name()`. Aks holda [[e0790-cannot-call-associated-function|E0790]] xato.
- **[[supertraits]]** — `trait OutlinePrint: fmt::Display` — `OutlinePrint`'ni implement qilmoqchi bo'lgan har tip `Display`'ni ham implementatsiya qilishi shart. Trait body'da `to_string()` ishlatish uchun zarur. Implementor unutsa: [[e0277-trait-bound-not-satisfied|E0277]].
- **[[newtype-pattern]]** — `struct Wrapper(InnerType)` orqali tashqi tipni mahalliy qilish. [[orphan-rule|Orphan rule]] tashqi trait'ni tashqi tip uchun implement qilishni man qiladi (`impl Display for Vec<T>` — E0117). Newtype bilan `Wrapper(Vec<String>)` mahalliy → istalgan trait. Compile time'da `Wrapper` qatlami yo'qoladi (zero overhead). Lekin inner method'lar avtomatik kelmaydi — qo'lda delegate yoki [[deref-trait|Deref]].
- Newtype faqat orphan rule chetlab o'tish emas: type safety (`Millimeters(u32)` ↔ `Meters(u32)`), API yashirish (`UserId(u64)`), va [[domain-modeling|domain modeling]] uchun ham.

Chapter 20.3 synthesis (Advanced Types):

- [[wiki/chapters/20-3-advanced-types|20.3]] type system'ning yana to'rt xususiyatini ochadi: [[newtype-pattern|newtype]] qayta ko'rib (encapsulation kontekstida), [[type-alias|type aliases]], [[never-type|never type (`!`)]], [[dynamically-sized-types|DST]] va [[sized-trait|`Sized`]].
- **Newtype qayta ko'rib:** 20.2'da orphan rule chetlab o'tish vositasi sifatida, 20.3'da yana ikki rol — (1) statik type safety (`Millimeters(u32)` ↔ `Meters(u32)`), (2) implementation yashirish (`People(HashMap<i32, String>)` ichidagi shakl noma'lum). Yengil [[encapsulation]].
- **[[type-alias|Type alias]]** (`type Name = ExistingType;`) — sinonim, yangi tip emas. `Kilometers = i32` da `Kilometers + i32` ishlaydi (newtype'dan farqli, type safety yo'q). Asosiy maqsad — uzun tip nomini qisqartirish: `type Thunk = Box<dyn Fn() + Send + 'static>;`. Standart kutubxonadagi klassik misol: `std::io::Result<T> = Result<T, std::io::Error>` — `Write` trait'i `Result<usize>` deb yozadi, lekin barcha `Result<T, E>` method'lari (`?`, `map`) ishlaydi.
- **[[never-type|Never type (`!`)]]** — qiymatga ega bo'la olmaydigan empty type. Funksiya hech qachon qaytmaganda return tipi: [[diverging-functions]] (`panic!`, `loop {}` break'siz, `process::exit`). `!` har tipga **coerce** bo'la oladi → `match` arm'lari bir xil tipda bo'lishi kerak qoidasi `panic!`/`continue` arm'i `T` tipdagi arm bilan birga ishlay olishini beradi. `unwrap` implementatsiyasi shu pattern'ga asoslangan: `Some(val) => val (T)` va `None => panic!() (!)` → match natijasi `T`.
- **[[dynamically-sized-types|DST]]** — compile-time'da o'lchami noma'lum tiplar: `str`, `[T]`, `dyn Trait`. **Oltin qoida:** har doim pointer orqali ishlatish (`&str`, `&[T]`, `Box<str>`, `&dyn Trait`). DST referensi — *fat pointer* (ptr + metadata: length yoki vtable). Compile-time'da fat pointer'ning o'lchami ma'lum (2 word).
- **[[sized-trait|`Sized` trait]]** — auto-derived marker. Generic funksiyalar default'da `T: Sized` (kompilyator implicit qo'shadi). `?Sized` cheklovni bo'shatadi: `fn print<T: ?Sized + Display>(t: &T)` — DST ham qabul qiladi (`t: T` → `t: &T`). `?Trait` sintaksisi **faqat `Sized` uchun**.
Chapter 20.4 synthesis (Advanced Functions and Closures):

- [[wiki/chapters/20-4-advanced-functions-and-closures|20.4]] Rust'da function va closure'larning advanced ishlatilishini ochadi: [[function-pointers|function pointers]], named functionlarni iterator callback sifatida uzatish, va [[returning-closures|closure qaytarish]].
- **`fn` vs `Fn`:** lowercase `fn(i32) -> i32` concrete function pointer type; `Fn`, `FnMut`, `FnOnce` esa [[fn-traits|closure traitlari]]. Function pointer uchala `Fn*` traitni implement qiladi, shuning uchun closure kutadigan joyga named function berish mumkin.
- API design qoidasi: agar faqat function pointer shart bo'lmasa, `fn(...)`dan ko'ra `F: Fn(...)` moslashuvchanroq. `F: Fn` named function ham, capturing closure ham qabul qiladi.
- [[ffi|FFI]] function pointer uchun asosiy maxsus holat: C closure tushunchasini bilmaydi, shuning uchun callback uchun ko'pincha plain function pointer kerak bo'ladi.
- `Iterator::map` closure bilan ham, named function bilan ham ishlaydi: `map(|i| i.to_string())` va `map(ToString::to_string)`. `ToString::to_string` [[fully-qualified-syntax|fully qualified syntax]] talab qiladi, chunki method nomi aniq trait kontekstiga bog'langan.
- [[enum-variants|Enum variant]] nomi initializer function sifatida ishlaydi: `Status::Value` `u32 -> Status` kabi ishlatiladi, shu sababli `(0u32..20).map(Status::Value).collect()` mumkin.
- Closure qaytarishda `impl Fn(i32) -> i32` bitta concrete closure typeni yashiradi. Bu [[opaque-types|opaque type]]: compiler type'ni biladi, caller faqat trait contractini ko'radi.
- Har closure alohida concrete type. Shuning uchun ikki function ikkalasi ham `impl Fn(i32) -> i32` qaytarsa ham, return typelari bir xil emas. Ularni bitta `Vec` ichida saqlashga urinish [[e0308-mismatched-types|E0308]] beradi.
- Common type kerak bo'lsa, [[trait-object|trait object]] ishlatiladi: `Box<dyn Fn(i32) -> i32>`. Bu heterogeneous closure handlers collectionini beradi, lekin heap allocation va [[dynamic-dispatch|dynamic dispatch]] narxini kiritadi.
Chapter 20.5 synthesis (Macros):

- [[wiki/chapters/20-5-macros|20.5]] Rust macro tizimini yakunlaydi. [[macro|Macro]] - compile-time'da code generatsiya qiladigan code, ya'ni [[metaprogramming]].
- Macro functiondan farq qiladi: function runtime'da ishlaydi va argument soni/type'lari signatureda aniq; macro compile-time'da expand bo'ladi, variable number of arguments qabul qilishi va trait implementation generatsiya qilishi mumkin. Narxi: definition murakkabroq va macro call oldin scope'da bo'lishi kerak.
- **[[declarative-macros|Declarative macros]]** `macro_rules!` bilan yoziladi. Ular source code structure patternlariga match qilib, replacement code generate qiladi. Simplified [[vec-macro|`vec!` macro]] patterni `( $( $x:expr ),* )`: `$x:expr` expression capture, `$()` repetition group, comma separator, `*` zero-or-more repetition.
- `vec![1, 2, 3]` soddalashtirilgan expansioni `Vec::new()`, uch marta `push`, va `temp_vec` return expressiondan iborat. Haqiqiy `vec!` allocationni oldindan qilish kabi optimizatsiyalar ham ishlatadi.
- **[[procedural-macros|Procedural macros]]** `TokenStream -> TokenStream` compile-time transformatsiyasi. Ular alohida `proc-macro` crate ichida turadi (`[lib] proc-macro = true`). Asosiy helper crates: `proc_macro` (compiler API), `syn` (parse), `quote` (Rust code tokenlari generatsiyasi).
- Procedural macro uch tur: [[custom-derive-macros|custom derive]] (`#[derive(HelloMacro)]`), [[attribute-like-macros|attribute-like]] (`#[route(GET, "/")]`), va [[function-like-macros|function-like]] (`sql!(...)`).
- `HelloMacro` derive example Rustda runtime reflection yo'qligini ko'rsatadi: type nomi (`Pancakes`) asosida `impl HelloMacro for Pancakes` code'i compile-time'da generatsiya qilinadi. `syn::parse` `DeriveInput` beradi, `ast.ident` type nomini chiqaradi, `quote!` generated code yaratadi, `stringify!` tokenni string literalga aylantiradi.
- Attribute-like macro signature: `#[proc_macro_attribute] fn route(attr: TokenStream, item: TokenStream) -> TokenStream`; `attr` attribute argumentlari, `item` esa target item.
- Function-like macro signature: `#[proc_macro] fn sql(input: TokenStream) -> TokenStream`; input oddiy Rust expression bo'lishi shart emas, shuning uchun DSL parsing uchun mos.
- Chapter 20 yakunlandi: unsafe, advanced traits, advanced types, advanced functions/closures, va macros birgalikda Rust'dagi kamroq ishlatiladigan, lekin kutubxona va frameworklar uchun muhim advanced toolboxni beradi.

Chapter 21 + 21.1 + 21.2 + 21.3 synthesis:

- [[wiki/chapters/21-final-project-building-a-multithreaded-web-server|Chapter 21]] oldingi mavzularni bitta real tizimga yig'adi: manual [[web-server]] qurish. Bu Rust Book'dagi uchinchi katta project chapter bo'lib, Chapter 2 va Chapter 12'dan keyingi amaliy yakuniy loyihadir.
- Mualliflar tayyor crate ishlatmaydi; maqsad raw [[tcp]] va [[http]] ustida ishlaydigan minimal server ichki mexanizmini ko'rish. Bu yerda `async/await` emas, thread-based yondashuv tanlangan, lekin ko'p async runtime'lar ham ichkarida worker thread'lar ishlatishi eslatiladi.
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|21.1]] ikki protocol qatlamini ajratadi: [[tcp]] bytes tashiydi, [[http]] esa request/response formatini belgilaydi. Ikkalasi ham bu kontekstda [[request-response]] modelida ishlaydi.
- `TcpListener::bind("127.0.0.1:7878")` va `incoming()` loop'i serverning kirish nuqtasini beradi. Har connection `TcpStream` bilan ifodalanadi; browser bitta sahifa uchun bir nechta connection ochishi mumkin.
- `BufReader::new(&stream)` + `lines()` pipeline'i HTTP request satrlarini o'qiydi. Request line `GET / HTTP/1.1` bo'yicha route tanlanadi; headers bo'sh satrgacha davom etadi.
- Minimal response `HTTP/1.1 200 OK\r\n\r\n` blank page qaytaradi; keyin `hello.html` fayli `fs::read_to_string` bilan o'qilib, `Content-Length` header bilan birga yuboriladi.
- `GET / HTTP/1.1` bo'lsa `hello.html`, aks holda `404.html` qaytarish orqali eng sodda routing quriladi. `(status_line, filename)` tuple destructuring refactori duplicated code'ni kamaytiradi.
- 21.2 bu bottleneck'ni `GET /sleep HTTP/1.1` bilan ko'zga ko'rinadigan qiladi: bitta sekin request butun single-threaded serverni ushlab qoladi.
- Birinchi concurrent qadam `thread::spawn(move || handle_connection(stream))` bilan har request uchun alohida thread yaratish, lekin bu operational jihatdan xavfli: cheksiz thread growth DoS yoki resource exhaustionga olib kelishi mumkin.
- To'g'ri yo'nalish - fixed-size [[thread-pool]]: `ThreadPool::new(4)` + `pool.execute(...)`. Chapter 21.2 bu API'ni avval yozib, keyin compiler xatolari bo'yicha implementation'ni quradi; bu [[compiler-driven-development]]ning aniq namunasi.
- Thread pool design'ining markazi: `type Job = Box<dyn FnOnce() + Send + 'static>`. `FnOnce` - job bir marta bajariladi, `Send` - worker threadga ko'chiriladi, `'static` - worker qancha vaqt ushlab turishi noma'lum.
- Std `mpsc` receiver single-consumer bo'lgani uchun `Receiver<Job>`ni workerlarga ownership bilan loop ichida tarqatib bo'lmaydi (`E0382`). Yechim: `Arc<Mutex<Receiver<Job>>>`.
- Juda nozik, lekin muhim detail: `let job = receiver.lock().unwrap().recv().unwrap();` lock'ni job bajarilishidan oldin qo'yib yuboradi. `while let Ok(job) = receiver.lock().unwrap().recv()` esa `MutexGuard`ni body oxirigacha ushlab qolib, butun pool'ni amalda seriallashtiradi.
- 21.3 shu yerdan keyingi real muammoni ko'rsatadi: "ishlaydigan" [[thread-pool]] hali "to'g'ri tugaydigan" pool emas. [[graceful-shutdown]] uchun server request olishni to'xtatgach, workerlar orderly ravishda chiqishi va main thread ularni kutishi kerak.
- Buning lifecycle hook'i [[drop]] bo'ladi: `ThreadPool` scope'dan chiqayotganda `Drop` impl ishga tushadi. Lekin `JoinHandle::join(self)` ownership olgani uchun `worker.thread.join()` borrowed field ustida ishlamaydi va [[e0507-cannot-move-out-of-borrowed-content|E0507]] beradi.
- Chapter 21.3 ownership extraction uchun ikki patternni ko'rsatadi: narrative fix sifatida `Vec::drain(..)`, final code sifatida esa `Option<JoinHandle<()>>` + `take()`. Ikkalasi ham cleanup ichida owned qiymatni borrowed struct ichidan xavfsiz chiqarish uchun.
- Channel bu bosqichda faqat queue emas, shutdown signal ham bo'ladi: `sender: Option<Sender<Job>>` va `drop(self.sender.take())` barcha senderlar yopilganini bildiradi.
- Worker tomonda `recv()` endi `unwrap()` qilinmaydi; `Err(_)` "endi yangi job yo'q" degan signal bo'lib, worker loop'dan `break` qiladi. Shundan keyin `join()` haqiqatan tugashi mumkin.
- `listener.incoming().take(2)` demo production pattern emas; u shunchaki shutdown sequence'ni tez ko'rsatish uchun ishlatiladi: ikki request, keyin pool drop, channel close, worker exit, join.

Chapter 22 + 22.1 synthesis:

- [[wiki/chapters/22-appendix|Chapter 22]] oddiy narrative chapter emas; u qolgan appendixlar Rust journey uchun reference material ekanini belgilaydi. Demak bu bo'limlar wiki'da lookup/reference layer sifatida ko'riladi.
- [[wiki/chapters/22-1-a-keywords|22.1]] lexical reference beradi: Rust keywordlari current va future reserved guruhlarga bo'linadi va odatda oddiy [[identifiers]] sifatida ishlatilmaydi.
- Eng amaliy takeaway [[raw-identifiers]]: `r#keyword` syntax'i keyword bilan naming conflictni hal qiladi. Canonical misol `fn r#match(...)` va `r#match(...)` call site.
- Raw identifiers faqat naming freedom emas, [[edition]] compatibility vositasi hamdir. Book'dagi `r#try` misoli 2015 edition library API'ni 2018/2021/2024 edition code'dan chaqirish uchun kerak bo'lishi mumkin.
- Appendix A shuningdek keywordlar existing concept xaritasini ham mustahkamlaydi: `async`/`await` -> [[async-await]] va [[future]], `dyn` -> [[trait-object]] va [[dynamic-dispatch]], `extern` -> [[extern-block]], `unsafe` -> [[unsafe-rust]], `pub` -> [[pub-keyword]], `where` -> [[where-clauses]], `crate` -> [[crate]], `match` -> [[match]].

Chapter 22.2 + 22.3 synthesis:

- [[wiki/chapters/22-2-b-operators-and-symbols|22.2]] Rust syntax'ni atlasga aylantiradi: operatorlar, path notation, generics notation, trait bounds, macros, comments, va qavslar bir reference qatlamiga yig'iladi.
- Eng practical syntax reference'lar: operator -> trait mapping ([[operator-overloading]]), expression context generic specialization `::<...>` yani [[turbofish]], va `r#"..."#` ko'rinishidagi [[raw-string-literals]].
- Appendix B bir belgi bir nechta rol o'ynashini aniq ko'rsatadi: `*` multiplication/dereference/raw pointer type, `!` macro call yoki logical complement, `..` range yoki struct update/pattern shorthand.
- [[wiki/chapters/22-3-c-derivable-traits|22.3]] esa `#[derive(...)]`ning standard-library reach'ini reference qiladi: [[debug-trait|Debug]], [[partial-eq|PartialEq]], [[eq-trait|Eq]], [[partial-ord]], [[ord-trait|Ord]], [[clone]], [[copy-trait|Copy]], [[hash-trait|Hash]], va [[default-trait|Default]].
- Eng muhim semantic chegaralardan biri: hamma trait derive qilinmaydi. `Display` user-facing format bo'lgani uchun compiler meaningful default bera olmaydi; shu sabab qo'lda implement qilinadi.
- Appendix C practical contractsni ham mustahkamlaydi: `assert_eq!` odatda [[partial-eq|PartialEq]] va [[debug-trait|Debug]] talab qiladi, [[eq-trait|Eq]] hash key semantics'iga ulanadi, [[ord-trait|Ord]] total ordering collectionlariga kerak bo'ladi, [[default-trait|Default]] esa `..Default::default()` va `unwrap_or_default` patternlarida yashaydi.

Chapter 22.4 + 22.5 + 22.6 + 22.7 synthesis:

- [[wiki/chapters/22-4-d-useful-development-tools|22.4]] Rustning official developer-tooling qatlamini ixcham qiladi: formatting uchun [[rustfmt]], compiler-driven mechanical fixlar uchun [[cargo-fix|cargo fix / rustfix]], qo'shimcha lintlar uchun [[clippy]], va editor intelligence uchun [[rust-analyzer]].
- Bu Appendix D batchi oldingi tool stublarni aniqroq joyiga qo'ydi: [[cargo|Cargo]] endi build system bo'lish bilan birga formatting/lint/fix orchestration nuqtasi ekanligi ko'rinadi, [[tooling]] esa bir nechta asbob qatlamlari orasidagi umbrella page bo'lib mustahkamlandi.
- [[wiki/chapters/22-5-e-editions|22.5]] muhim boundary'ni aniq ajratadi: compiler version boshqa narsa, [[edition]] esa source qaysi compatibility qoidalari bilan parse qilinishini bildiradi. Shu sabab Rust yangi syntax va rules'ni ecosystemni sindirmasdan bosqichma-bosqich kiritadi.
- Editions opt-in bo'lsa ham ecosystem fragmentatsiyasi paydo qilmaydi: turli editiondagi crates bir-biri bilan link bo'la oladi. Migration workflowida [[edition-migration]] va [[cargo-fix-edition-migration]] practical bridge bo'lib xizmat qiladi.
- [[wiki/chapters/22-6-f-translations-of-the-book|22.6]] knowledge-management nuqtasidan live registry emas, balki source-time snapshot. Wiki ichida [[rust-book-translations]] sahifasi aynan shu snapshotni saqlaydi; unda Uzbek translation alohida qayd etildi, lekin aktual holat web verification'siz tasdiqlanmagan.
- [[wiki/chapters/22-7-g-how-rust-is-made-and-nightly-rust|22.7]] Rustning "stability without stagnation" modelini yakuniy reference qatlamga aylantiradi: [[release-channels]] (`stable`, `beta`, `nightly`), nightly-only [[feature-flags]], `rustup override` bilan per-project toolchain, va language evolution uchun [[rust-rfc-process]].
- Shu to'rtta appendix birga qaralganda Appendix 22 faqat syntax/reference qo'shimchasi emasligi ko'rinadi; u Rust ecosystem'ning operational layerini ham jamlaydi: kodni qanday formatlash, lintlash, migrate qilish, qaysi editionda o'qish, qaysi channelda sinash, va knowledge'ni qaysi historical snapshot sifatida saqlash.

Rust for Backend Developers `1. intro` synthesis:

- [[wiki/chapters/rust-for-backend-developers-1-intro|1. Intro]] bu source oilasini beginner Rust course emas, balki experienced back-end developers uchun fast onboarding sifatida joylaydi. O'quvchi [[http|HTTP]], relational databases, JSON, [[threads|multithreading]], va [[stack-and-heap|stack/heap]] kabi mavzularni oldindan biladi deb qabul qilinadi.
- Setup boblari Rust installni ikki qatlamga ajratadi: [[rustup]] bilan Rust toolchain va platformaga bog'liq [[c-toolchain|C/C++ toolchain]]/[[linker]] qatlami. Windowsda [[msvc-toolchain|MSVC]] default yo'l, [[mingw|MinGW]] esa GNU target alternativasi; Linuxda GCC + rustup ketma-ketligi ko'rsatiladi.
- Development environment bobida [[rust-analyzer]] va [[language-server-protocol|Language Server Protocol]] editor feedback loop markaziga qo'yiladi. [[vscode|VSCode]] extension talab qiladi, [[zed-editor|Zed]] esa rust-analyzer bilan tayyor keladi; [[rust-rover|Rust Rover]] alohida IDE/analyzer modeliga ega.
- First Look Rust Bookdagi [[hello-world]] materialini backend kitob kontekstida takrorlaydi: `fn main`, [[println-macro|println! macro]], [[rustc]] bilan single-file compile, va OSga bog'liq run command.
- Safe Rust bobi [[memory-safety]] va [[unsafe-rust]] chegarasini erta belgilaydi. Muhim tuzatish: source memory leakni safe Rust oldini oladigan xatolar ro'yxatiga kiritadi, lekin wiki'da bu ehtiyotkorroq yozildi - safe Rust UB va memory unsafetyni oldini oladi, leaks esa alohida resource-management muammosi bo'lishi mumkin.

Rust for Backend Developers `2. base` synthesis:

- [[wiki/chapters/rust-for-backend-developers-2-base|2. Base]] Rust syntaxini reference-style foundationga aylantiradi: bindings, primitive types, formatting, block expressions, references, arrays, vectors, va slices bir section ichida jamlangan.
- Variables bobi [[variables-and-mutability|variables and mutability]]ni beginner darajada tozalaydi: `let`, delayed initialization, [[immutability]], `mut`, [[constants]], [[static-items]], [[raw-identifiers]], va [[discarded-binding]] bir joyga yig'iladi.
- Primitive types bobi `i32`/`f64` default inference, Unicode `char`, singleton [[unit-type|unit type]], conceptual [[never-type|never type (!)]], va explicit [[type-casting]] orqali Rustning "no hidden conversions" tip intizomini ko'rsatadi.
- Console output bobi [[println-macro|println! macro]]ni shunchaki log chiqarish vositasi emas, balki [[display-formatting|Display formatting]] va [[debug-formatting|Debug formatting]] boundarysiga kirish nuqtasi sifatida ko'rsatadi.
- Scopes bobi [[scope]]ni ownership oldidan ham muhim qiladi: block variable lifetime'ini cheklaydi va bir vaqtning o'zida value ham qaytaradi; bu [[statements-and-expressions|statements and expressions]] va `()` modeliga tayanch.
- References, arrays, vector, va slices ketma-ketligi contiguous memory mental modelini yig'adi: `[T; N]` compile-time shape, [[vector|Vec<T>]] runtime shape, [[slices]] esa ownershipsiz window. Shu yerning o'zida borrowing va reallocation haqidagi keyingi ownership materiallari uchun zamin tayyorlanadi.
- Strings bobi shu foundation'ga textni olib kiradi: [[string-type|String]] owned heap buffer, [[string-slice|String slice]] borrowed UTF-8 view, [[format-macro|format! macro]] esa text composition'ning owned natijasini beradi.
- `if` va loops bo'limlari control flow'ni yana ham aniqroq expression modeliga bog'laydi: [[if-expressions|if expressions]] branch result qaytaradi, [[loop]] esa `break value` bilan natija bera oladi, [[for-loop|for loop]] esa iterator/range traversal uchun Rustning default yo'li.
- Functions va tuples bo'limlari typed syntaxni data-flow bilan bog'laydi: final expression return, explicit `return`, conceptual [[never-type|never type (!)]], multiple valuesni [[tuple]] orqali qaytarish, va `&[T]` parameter bilan array hamda vector uchun bitta API yozish.
- Structs bo'limi foundation'ni domain modelingga olib boradi: [[structs]] named fields beradi, [[struct-update-syntax]] ownershipni ko'rinadigan qiladi, [[methods]] va [[impl-block|impl block]] esa behaviorni type yoniga olib keladi. Tuple struct va unit-like structlar esa "struct = faqat record" degan tor tasavvurni kengaytiradi.
- Ownership va lifetimes bo'limlari `2. base`ni haqiqiy Rust mental modeliga yakunlaydi: [[ownership]] move va cleanupni boshqaradi, [[borrow-checker]] reference safety'ni ushlaydi, [[lifetimes]] esa reference relation'larini signature darajasida ifodalaydi. Shu joydan boshlab `&str` vs `String`, vector reallocation, va borrow conflictlar tasodifiy qoidalar emas, bitta tizim bo'lib ko'rina boshlaydi.
- Declarative macro va pointer bo'limlari `2. base`ni safe surface'dan bir qadam tashqariga olib chiqadi: bir tomonda [[declarative-macros|macro_rules!]] bilan compile-time code generation, ikkinchi tomonda [[raw-pointer]] va [[unsafe-rust|unsafe Rust]] bilan low-level escape hatch. Shu ikki bo'lim birga Rustning ikkita kuchli, lekin ehtiyotkor ishlatilishi kerak bo'lgan qirrasini beradi.
- Modules bo'limi shu foundation'ni code organization bilan tugatadi: [[module-system|module system]], [[pub-keyword|pub keyword]], [[use-declarations|use declarations]], va [[crate]] orqali Rust project bitta file'dan oshganda qaysi mental model bilan o'sishini ko'rsatadi.
- Traits bo'limi esa shu foundation'ni abstraction qatlami bilan yopadi: [[traits]] behavior contractini beradi, [[static-dispatch|static dispatch]] `impl Trait` va [[monomorphization]] bilan compile-time specialization beradi, [[trait-object|trait object]] esa [[dynamic-dispatch|dynamic dispatch]] orqali runtime polymorphism ochadi. Shu yerda [[orphan-rule|orphan rule]], default impl, supertraits, `Self`, va [[unsafe-trait|unsafe trait]] bir joyga tushadi.
- Auto-derive bo'limi data-carrying type'larni trait semanticsiga ulaydi: `#[derive(...)]` [[attribute]] sifatida ishlaydi, [[partial-eq|PartialEq]] equality'ni, [[clone]] explicit duplication'ni, [[copy-trait|Copy trait]] esa implicit copy semanticsni ajratadi. Shu joyda [[marker-trait|marker trait]] degan signal-model ham aniq ko'rinadi.
- Destructuring va pattern matching bo'limlari `2. base`ni syntaxdan semantikaga olib o'tadi: tuple/array/struct/function-param [[pattern-destructuring|destructuring]]i, keyin esa [[match]] orqali range, [[at-binding|@ binding]], [[match-guard|match guard]], [[slice-patterns|slice patterns]], va [[ref-pattern|ref pattern]] bilan branch + binding modeli yakunlanadi.
- Anonymous functions bo'limi shu foundation'ga callable values qatlamini qo'shadi: non-capturing callable [[function-pointers|function pointer]] bo'lishi mumkin, capturing callable esa [[closures]] bo'ladi. Shu yerda [[higher-order-functions|higher-order functions]], [[fn-traits|Fn traits]], `move` capture, `impl Fn` opaque return, va `Box<dyn Fn>` dynamic dispatch yechimi bitta amaliy chiziqqa tushadi.
- Generics bo'limi `2. base`ni compile-time abstraction bilan to'ldiradi: `Holder<T>` template modeli, [[monomorphization]], [[turbofish]], generic trait vs [[associated-types|associated types]], [[trait-bounds|trait bounds]], [[where-clauses|where clauses]], va [[const-generics|const generics]] Rustning "parametrizatsiya, keyin specialization" mental modelini yig'adi. Shu yerda `impl Trait` nested `Fn` return boundning universal o'rnini bosa olmasligi ham ochiq ko'rsatiladi.
- Enums, Option, va Result bo'limlari `2. base`ni "shape va failure" qatlami bilan tugatadi: [[enums]] payloadli ADT sifatida keladi, [[if-let|if let]] single-variant branch beradi, [[option|Option]] absence'ni type systemga ko'taradi, [[result|Result]] esa recoverable errorni explicit qiladi. Shu yerda `map`/`flatten`/`and_then` combinatorlari, [[std-error-trait|std::error::Error]], [[question-mark-operator|question mark operator]], va ignored `Result` uchun [[discarded-binding]] bir joyga tushadi.
- [[wiki/chapters/rust-for-backend-developers-iterators|Iterators]] va [[wiki/chapters/rust-for-backend-developers-smart-pointers|Smart Pointers]] `2. base`ni traversal hamda heap ownership toolkit bilan kengaytiradi: `for` aslida [[into-iterator|IntoIterator]] orqali ishlashi, [[iterators]] lazy bo'lishi, [[box-t|Box<T>]] recursive type sizing berishi, [[rc-t|Rc<T>]] shared ownership ochishi, [[cell-t|Cell<T>]] va [[refcell-t|RefCell<T>]] esa single-threaded interior mutabilityning ikki boshqa modelini berishi shu yerda aniq ko'rinadi.
- Cargo va dependency bo'limlari `2. base`ni syntax/reference sectiondan real project workflow'iga olib chiqadi: [[cargo|Cargo]], [[cargo-toml|Cargo.toml]], [[dependencies]], [[crates-io|crates.io]], [[cargo-features]], va [[semver|SemVer]] endi code yozishdan tashqari package boshqarish mental modelini beradi.
- `package`, `crate`, `module` farqi shu bosqichda yopiladi: [[package]] Cargo container, [[crate]] compile unit, [[module]] esa code organization boundary; `src/main.rs`, `src/lib.rs`, va [[src-bin|src/bin]] conventions bu modelni filesystem darajasida ko'rinadigan qiladi.
- Workspace va testing bo'limlari sectionni team-scale practice bilan yakunlaydi: [[cargo-workspaces|cargo workspaces]], [[workspace-dependencies]], `cargo run -p`, [[testing]], [[unit-tests|unit tests]], [[integration-tests|integration tests]], [[dev-dependencies]], trait-based mock, va [[cargo-nextest]] backend codebase'ni build/test qilishda asosiy operational qatlamni beradi.

Rust for Backend Developers `3. advance` synthesis:

- [[wiki/chapters/rust-for-backend-developers-3-advance|3. Advance]] sectioni syntax va beginner ergonomics'dan keyingi semantic contract qatlamiga o'tadi.
- [[wiki/chapters/rust-for-backend-developers-common-traits|Common Traits]] equality, ordering, conversion, borrowing, cleanup, DST, va thread-safety markerlarini standard traitlar orqali tartiblaydi.
- `Eq` vs `PartialEq`, `Ord` vs `PartialOrd`, `From` vs `Into`, `AsRef` vs `Borrow`, va `T: Sized` vs `T: ?Sized` farqlari bu sectionning asosiy mental modeli bo'lib chiqadi.
- Keyingi qatlamda [[wiki/chapters/rust-for-backend-developers-multithreading|Multithreading]] shu marker traitlarni amaliy execution modelga ulaydi: `thread::spawn`, [[join-handle]], [[thread-builder]], va `FnOnce + Send + 'static` boundary'si endi real operational ma'no oladi.
- Shared state bu yerda bitta pattern emas: [[mutex-t|Mutex<T>]] exclusive access beradi, [[rwlock|RwLock]] read-heavy workload'ni ochadi, [[condvar|Condvar]] signal/wait coordination beradi, [[barrier|Barrier]] esa phase synchronization uchun ishlaydi. Shu joyda [[poisoned-mutex]] va [[mutexguard-lifetime]] kabi nozik, lekin real bug manbalari ham ochiladi.
- [[scoped-threads]] ordinary `spawn` local borrow'ni qabul qilmaydigan joyda `thread::scope` `Arc` va `move`ni chetlab o'tadi, lekin background task modeli emas, scope-bound parallelism ekanini saqlaydi.
- [[atomic-types]] `Mutex<T>`ning o'rnini bosadigan universal vosita emas. Counter, flag, va tor state uchun qulay, lekin [[atomic-memory-ordering]] tanlovi noto'g'ri bo'lsa race-free bo'lsa ham visibility mantiqi buzilishi mumkin. Shu sabab `[[ordering|Ordering]]` sahifasidagi compare enum bilan aralashtirilmaydigan alohida memory-model sahifa ochildi.
- [[compare-exchange]] CAS loop logikasini aniq qiladi: oddiy `load + store` concurrent update uchun yetmaydi; `expected -> new` modeli failure'da actual value'ni qaytarib retry qilishni talab qiladi.
- [[thread-local-storage]] `Cell<T>`ni boshqa kontekstda qayta joylashtiradi: shared `static`da `Cell<T>` `Sync` emas, lekin TLS ichida har thread o'z nusxasi bilan ishlaydi.
- [[channels]] bo'limi endi faqat ownership transfer emas, lifecycle coordination sifatida ham ko'riladi. Source'dagi `Element::Finish` variant yuborilmagani alohida qayd qilindi: loop sender close sabab tugaydi. [[sync-channel]] esa bounded queue orqali backpressure modelini qo'shadi.
- [[wiki/chapters/rust-for-backend-developers-global-data|Global Data]] shu concurrency foundation ustiga global initialization qatlamini qo'yadi: [[constants]] compile-time only, [[static-items]] bitta shared allocation, [[mutable-static]] esa unsafe escape hatch. Safe defaultlar sifatida [[lazylock|LazyLock]] va [[oncelock|OnceLock]] ajratildi.
- Session storage misoli `global registry lock` va `object lock` farqini amaliy ko'rsatadi: tashqi `RwLock<HashMap<...>>` qisqa ushlanadi, ichki `Arc<Mutex<UserSession>>` esa individual session mutation uchun ishlatiladi. Bu pattern qulay, lekin global state'ni yaxshi design deb avtomatik oqlamaydi.
- [[wiki/chapters/rust-for-backend-developers-error-handling|Error Handling]] sectionni app/library boundary bo'yicha bo'ladi: [[custom-error-enum]] va [[wiki/crates/thiserror|thiserror]] public/domain API uchun, [[box-dyn-error|Box<dyn Error>]] va [[wiki/crates/anyhow|anyhow]] esa typed recovery kerak bo'lmagan app-level boundary uchun.
- [[error-wrapping]] va `#[from]` bilan `?` birga ishlaganda lower-level service errors higher-level API errors'ga o'tadi. Source snippet'dagi `servation` va `(0)` artefaktlari canonical syntax sifatida emas, source typo sifatida qayd qilindi.
- [[error-context]] va [[root-cause]] bir xil narsa emas: root cause asli failure, context esa qaysi operation yiqilganini aytadi. `anyhow` shu ikkisini birga ko'taradi va kerak bo'lsa backtrace ham beradi.
- [[wiki/chapters/rust-for-backend-developers-serialization|Serialization]] backend boundary'ni data format tomondan yopadi: [[wiki/crates/serde|serde]] format emas, `Serialize`/`Deserialize` trait layer; [[wiki/crates/serde-json|serde_json]] va [[wiki/crates/serde-xml-rs|serde-xml-rs]] esa format crate'lari.
- Serialization bo'limida enum tagging va field rename public API contract sifatida qaraladi. `serde_json::Value` dynamic JSON inspection uchun bor, lekin typed DTO defaultini almashtirmaydi. `DeserializeOwned` generic parsingda input lifetime'dan mustaqil owned result talabini beradi.
- [[wiki/chapters/rust-for-backend-developers-date-and-time|Date and Time]] elapsed timing va wall-clock timestampni ajratadi: [[instant|Instant]] measurement uchun, [[system-time|SystemTime]] Unix timestamp uchun, [[duration|Duration]] esa interval uchun ishlatiladi.
- [[wiki/crates/chrono|chrono]] qatlami timezone modelini ochadi: [[naive-date-time|NaiveDateTime]] exact instant emas, [[date-time|DateTime<Tz>]] timezone context saqlaydi, [[rfc3339|RFC3339]] esa API timestamp uchun aniq wire format beradi.
- [[wiki/chapters/rust-for-backend-developers-logging|Logging]] `3. advance`ni observability boundary bilan kengaytiradi: [[wiki/crates/tracing|tracing]] event/span API, [[wiki/crates/tracing-subscriber|tracing-subscriber]] subscriber/filter/layer implementation, [[wiki/crates/tracing-appender|tracing-appender]] esa rolling va non-blocking output uchun ishlatiladi.
- Logging bo'limida [[rust-log|RUST_LOG]] runtime filter policy sifatida, [[tracing-span|tracing span]] esa request/function contextni loglarga bog'lash vositasi sifatida ajratildi. Async `.await` boundary bilan entered span scope ehtiyotkorlik talab qilishi alohida qayd qilindi.
- [[wiki/chapters/rust-for-backend-developers-application-configuration|Application Configuration]] startup boundary'ni ko'rsatadi: [[wiki/crates/config|config]] file va env sourcelarni merge qiladi, [[wiki/crates/serde|serde]] esa final config tree'ni typed structga aylantiradi.
- Configuration bo'limi `default.toml` + profile TOML + env override orderini asosiy mental model qiladi. Source passwordlarni TOMLda ko'rsatsa ham, wiki production secretlarni plain tracked config file'da saqlashni xavfli deb belgilaydi.

Rust for Backend Developers `4. async` synthesis:

- [[wiki/chapters/rust-for-backend-developers-4-async|4. Async]] sectioni Rust async modelini user-space execution flow sifatida ochishni boshladi.
- [[wiki/chapters/rust-for-backend-developers-async-in-rust|Async in Rust]] `async fn` chaqiruvi natijani emas, [[future|Future]] qaytarishini aniq qiladi; natijani olish uchun [[executor]] yoki [[async-runtime|async runtime]] kerak.
- [[wiki/crates/futures|futures]] source'da sodda `futures::executor::block_on` demo uchun ishlatiladi. Bu high-load backend runtime emas, executor zarurligini ko'rsatuvchi kichik kirish nuqtasi.
- `.await` sequential compositionni callback zanjirisiz yozdiradi, lekin aslida suspension point: future `Pending` bo'lsa [[waker|Waker]] signali kelganda qayta `poll` qilinadi.
- [[task-context|Context]], [[poll-enum|Poll]], [[waker|Waker]], va [[pin|Pin]] endi async trait contractining alohida tushunchalari sifatida ajratildi.
- Source'dagi "fiber" atamasi Rust rasmiy public API nomi emas; wiki uni [[fiber]] sahifasida faqat [[future|Future]] va async state machine uchun mental model sifatida saqlaydi.
