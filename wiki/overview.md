---
title: "Rust Wiki Overview"
type: overview
status: draft
created: 2026-05-06
updated: 2026-05-07
tags: [rust, overview]
source_count: 59
---

# Rust Wiki Overview

This wiki is a living Rust learning reference. It will grow from book chapters, personal notes, examples, exercises, and questions.

Current focus:

- Build a durable mental model of Rust.
- Keep Rust terms in English while explaining in Uzbek.
- Connect chapters to reusable concept pages.
- Save useful answers back into the wiki so knowledge compounds.

Ingested Rust Book materials:

- [[0-the-rust-programming-language-the-rust-programming-language|0. The Rust Programming Language]]
- [[0-1-foreword-the-rust-programming-language|0.1. Foreword]]
- [[0-2-introduction-the-rust-programming-language|0.2. Introduction]]
- [[1-getting-started-the-rust-programming-language|1. Getting Started]]
- [[1-1-installation-the-rust-programming-language|1.1. Installation]]
- [[1-2-hello-world-the-rust-programming-language|1.2. Hello, World!]]
- [[1-3-hello-cargo-the-rust-programming-language|1.3. Hello, Cargo!]]
- [[2-programming-a-guessing-game-the-rust-programming-language|2. Programming a Guessing Game]]
- [[3-common-programming-concepts-the-rust-programming-language|3. Common Programming Concepts]]
- [[3-1-variables-and-mutability-the-rust-programming-language|3.1. Variables and Mutability]]
- [[3-2-data-types-the-rust-programming-language|3.2. Data Types]]
- [[3-3-functions-the-rust-programming-language|3.3. Functions]]
- [[3-4-comments-the-rust-programming-language|3.4. Comments]]
- [[3-5-control-flow-the-rust-programming-language|3.5. Control Flow]]
- [[4-understanding-ownership-the-rust-programming-language|4. Understanding Ownership]]
- [[4-1-what-is-ownership-the-rust-programming-language|4.1. What Is Ownership?]]
- [[4-2-references-and-borrowing-the-rust-programming-language|4.2. References and Borrowing]]
- [[4-3-the-slice-type-the-rust-programming-language|4.3. The Slice Type]]
- [[5-using-structs-to-structure-related-data-the-rust-programming-language|5. Using Structs to Structure Related Data]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language|5.1. Defining and Instantiating Structs]]
- [[5-2-an-example-program-using-structs-the-rust-programming-language|5.2. An Example Program Using Structs]]
- [[5-3-methods-the-rust-programming-language|5.3. Methods]]
- [[6-enums-and-pattern-matching-the-rust-programming-language|6. Enums and Pattern Matching]]
- [[6-1-defining-an-enum-the-rust-programming-language|6.1. Defining an Enum]]
- [[6-2-the-match-control-flow-construct-the-rust-programming-language|6.2. The match Control Flow Construct]]
- [[6-3-concise-control-flow-with-if-let-and-let-else-the-rust-programming-language|6.3. Concise Control Flow with if let and let...else]]
- [[7-packages-crates-and-modules-the-rust-programming-language|7. Packages, Crates, and Modules]]
- [[7-1-packages-and-crates-the-rust-programming-language|7.1. Packages and Crates]]
- [[7-2-control-scope-and-privacy-with-modules-the-rust-programming-language|7.2. Control Scope and Privacy with Modules]]
- [[7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language|7.3. Paths for Referring to an Item in the Module Tree]]
- [[7-4-bringing-paths-into-scope-with-the-use-keyword-the-rust-programming-language|7.4. Bringing Paths Into Scope with the use Keyword]]
- [[7-5-separating-modules-into-different-files-the-rust-programming-language|7.5. Separating Modules into Different Files]]
- [[8-common-collections-the-rust-programming-language|8. Common Collections]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language|8.1. Storing Lists of Values with Vectors]]
- [[8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language|8.2. Storing UTF-8 Encoded Text with Strings]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language|8.3. Storing Keys with Associated Values in Hash Maps]]
- [[9-error-handling-the-rust-programming-language|9. Error Handling]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language|9.1. Unrecoverable Errors with panic!]]
- [[9-2-recoverable-errors-with-result-the-rust-programming-language|9.2. Recoverable Errors with Result]]
- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language|9.3. To panic! or Not to panic!]]
- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language|10. Generic Types, Traits, and Lifetimes]]
- [[10-1-generic-data-types-the-rust-programming-language|10.1. Generic Data Types]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language|10.2. Defining Shared Behavior with Traits]]
- [[10-3-validating-references-with-lifetimes-the-rust-programming-language|10.3. Validating References with Lifetimes]]
- [[11-writing-automated-tests-the-rust-programming-language|11. Writing Automated Tests]]
- [[11-1-how-to-write-tests-the-rust-programming-language|11.1. How to Write Tests]]
- [[11-2-controlling-how-tests-are-run-the-rust-programming-language|11.2. Controlling How Tests Are Run]]
- [[11-3-test-organization-the-rust-programming-language|11.3. Test Organization]]
- [[13-functional-language-features-the-rust-programming-language|13. Functional Language Features: Iterators and Closures]]
- [[13-1-closures-the-rust-programming-language|13.1. Closures]]
- [[13-2-processing-a-series-of-items-with-iterators-the-rust-programming-language|13.2. Processing a Series of Items with Iterators]]
- [[13-3-improving-our-io-project-the-rust-programming-language|13.3. Improving Our I/O Project]]
- [[13-4-performance-in-loops-vs-iterators-the-rust-programming-language|13.4. Performance: Comparing Loops and Iterators]]
- [[14-more-about-cargo-and-crates-io-the-rust-programming-language|14. More About Cargo and Crates.io]]
- [[14-1-customizing-builds-with-release-profiles-the-rust-programming-language|14.1. Customizing Builds with Release Profiles]]
- [[14-2-publishing-a-crate-to-crates-io-the-rust-programming-language|14.2. Publishing a Crate to Crates.io]]
- [[14-3-cargo-workspaces-the-rust-programming-language|14.3. Cargo Workspaces]]
- [[14-4-installing-binaries-with-cargo-install-the-rust-programming-language|14.4. Installing Binaries with cargo install]]
- [[14-5-extending-cargo-with-custom-commands-the-rust-programming-language|14.5. Extending Cargo with Custom Commands]]

Current source baseline:

- Rust Book material assumes Rust `1.90.0` or later.
- Project examples should be read with `edition = "2024"` in `Cargo.toml`.
- Official Rust Book can be used online or offline through `rustup doc --book`.

Current learning frame:

- Rust aims to combine high-level ergonomics with low-level control.
- Rust's compiler is part of the learning and development workflow, not just a final checker.
- The book mixes concept chapters with project chapters; Chapter 2, Chapter 12, and Chapter 21 are the main project chapters.
- Compiler error messages should be collected into `wiki/errors/` once concrete examples appear.

Chapter 1 synthesis:

- [[1-getting-started|1. Getting Started]] establishes the local workflow: install with [[rustup]], verify with [[rustc]], write [[hello-world]], then switch to [[cargo|Cargo]].
- Direct `rustc` is useful for seeing compile/run mechanics, but Cargo is the normal Rust project workflow.
- `cargo check` is the fast feedback command; `cargo build --release` is for optimized release binaries and benchmarks.

Chapter 2 synthesis:

- [[2-programming-a-guessing-game|2. Programming a Guessing Game]] introduces Rust concepts through a project rather than a reference-style explanation.
- The project practices [[variables-and-mutability|variables and mutability]], [[result|Result]], [[match]], [[loop]], [[shadowing]], [[crate]], and [[rand]].
- The first saved compiler diagnostic is [[e0308-mismatched-types|E0308 mismatched types]], caused by comparing `String` input with a numeric secret value.
- Chapter 3 now explains those concepts more systematically.

Chapter 3 synthesis:

- [[3-common-programming-concepts|3. Common Programming Concepts]] turns Chapter 2's hands-on concepts into a reference foundation.
- Rust defaults to immutable variables, supports explicit `mut`, and uses [[shadowing]] for transformations that may change type.
- [[data-types|Data types]] are known at compile time; ambiguity such as `"42".parse()` needs explicit type annotations.
- Rust functions are expression-oriented: final expressions return values, while semicolons turn expressions into statements.
- [[control-flow|Control flow]] requires explicit Boolean conditions and favors `for` loops for safe collection iteration.
- New saved diagnostics: [[e0284-type-annotations-needed|E0284 type annotations needed]] and [[e0384-cannot-assign-twice|E0384 cannot assign twice]].

Chapter 4 synthesis:

- [[4-understanding-ownership|4. Understanding Ownership]] starts Rust's unique memory model.
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

- [[5-using-structs-to-structure-related-data|5. Using Structs to Structure Related Data]] Rustda related data'ni named custom types bilan model qilishni boshlaydi.
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

- [[6-enums-and-pattern-matching|6. Enums and Pattern Matching]] Rustning value "qaysidir bittasi" tabiatiga ega bo'lgan domenlarni modellash imkoniyatini ochadi.
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

- [[7-packages-crates-and-modules|7. Packages, Crates, and Modules]] katta Rust projectlarni tartiblash muammosini ochadi: code bitta file va bitta [[module]]dan bir nechta modules, files, [[crate|crates]], va [[package|packages]]ga o'sadi.
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

- [[8-common-collections|8. Common Collections]] standard librarydagi runtime grow/shrink qiladigan heap-backed [[collections|collections]] mavzusini ochadi.
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

- [[9-error-handling|9. Error Handling]] Rustda xatolarni explicit design decision sifatida ko'rsatadi: [[recoverable-errors|recoverable errors]] uchun [[result|Result<T, E>]], [[unrecoverable-errors|unrecoverable errors]] uchun [[panic|panic!]].
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

- [[10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]] Rust abstraction toolsini ochadi: [[generics]], [[traits]], va [[lifetimes]].
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

- [[11-writing-automated-tests-the-rust-programming-language|Chapter 11]] Rust testlash mexanikasini ochadi: annotatsiyalar/macrolar (11.1), test ishlatish optsiyalari (11.2), unit va integration tests tashkiloti (11.3).
- [[testing|Testing]] Rustda type system va borrow checker yetib bormagan mantiqiy xatolarni ushlaydi; Dijkstra: "testlar buglar borligini ko'rsatadi, yo'qligini isbotlamaydi."
- [[cfg-test|`#[cfg(test)]`]] faqat `cargo test`da compile bo'ladigan `mod tests` blokini belgilaydi; release binary'ga test kodi kirmaydi.
- [[test-macros|Test macro'lar]]: `assert!` boolean tekshiradi; `assert_eq!` / `assert_ne!` tenglik/tengsizlik tekshirib, xatoda ikkala qiymatni chiqaradi — `PartialEq + Debug` talab qiladi.
- Custom failure messages — `assert!`, `assert_eq!`, `assert_ne!`ga format string qo'shimcha argument sifatida beriladi.
- [[should-panic|`#[should_panic]`]] kodning panic qilishini testlaydi; `expected = "substring"` bilan aniqlashtirish mumkin.
- `Result<T, E>` returning testlar `?` operator ishlatishga imkon beradi; `#[should_panic]` bilan mos kelmaydi.
- 11.2 [[11-2-controlling-how-tests-are-run-the-rust-programming-language|Controlling How Tests Are Run]] test binary argumentlarini `--` bilan ajratishni o'rgatdi.
- Default parallel ishlatish shared state muammolariga olib kelishi mumkin; `--test-threads=1` ketma-ket ishlatish beradi.
- O'tgan testlar outputi default'da ushlanadi; `-- --show-output` bilan ko'rsatish mumkin.
- [[test-filtering|Test filtrlash]]: `cargo test <pattern>` — nomi substringga mos barcha testlar ishlaydi; modul nomi ham qidiriladi.
- `#[ignore]` og'ir testlarni standart rundan chiqaradi; `-- --ignored` va `-- --include-ignored` maxsus holatlarda ishlatiladi.
- 11.3 [[11-3-test-organization-the-rust-programming-language|Test Organization]] ikki kategoriyani rasmiylashtirdi: [[unit-tests]] va [[integration-tests]].
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
- [[fn-traits|Fn traits]] — additive pog'onali hierarchy: `FnOnce` (bir marta, captured qiymat move bo'lsa), `FnMut` (bir necha marta, mutatsiya bo'lishi mumkin), `Fn` (bir necha marta, concurrent xavfsiz). Barcha closures kamida `FnOnce` implement qiladi.
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

- [[14-more-about-cargo-and-crates-io|Chapter 14]] Cargo'ning ilg'or imkoniyatlarini ochadi: release profillari, crates.io'ga publish qilish, workspaces, `cargo install`, va custom subcommandlar.
- 14.1: [[release-build|Release profiles]] — `dev` (`opt-level = 0`, tez compile) va `release` (`opt-level = 3`, tez runtime). `Cargo.toml`'da `[profile.*]` bo'limlari orqali override qilish mumkin.
- 14.2: [[documentation-comments|Doc comments]] — `///` keyingi item, `//!` container (crate/module) uchun; `cargo doc --open` HTML generatsiya qiladi. `# Examples` bloki `cargo test` bilan doc-test sifatida ishga tushadi — dokumentatsiya va kod sinxronligini ta'minlaydi. [[pub-use|pub use]] ichki modul tuzilmasidan mustaqil, foydalanuvchi uchun qulay public API yaratishga imkon beradi.
- 14.2: [[crates-io|crates.io]] publish workflow — `cargo login`, majburiy metadata (`description`, `license` SPDX), `cargo publish` (mangu, o'chirib bo'lmaydi), `cargo yank` (yangi loyihalarga bloklash, mavjudlarni buzmasdan).
- 14.3: [[cargo-workspaces|Cargo workspaces]] — bitta `Cargo.lock` va `target/` ostida bir nechta package; ichki dependency `path = ...` bilan explicit; tashqi dependency har crate'da alohida; `cargo run/test -p name`.
- 14.4: `cargo install` binary target bo'lgan crate'larni `~/.cargo/bin/` ga o'rnatadi; `$PATH`'da bo'lishi kerak.
- 14.5: `cargo-something` naming pattern — `cargo something` sifatida chaqirish; `cargo --list` barcha subcommandlarni ko'rsatadi.
- Chapter 14 to'liq yakunlandi. Keyingi: Chapter 15 (Smart Pointers).
