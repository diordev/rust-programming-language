---
title: "10. Generic Types, Traits, and Lifetimes - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, generics, traits, lifetimes]
source_count: 1
source_path: "raw/books/the_rust_programming_language/10. Generic Types, Traits, and Lifetimes - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch10-00-generics.html"
---

# 10. Generic Types, Traits, and Lifetimes - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/10. Generic Types, Traits, and Lifetimes - The Rust Programming Language.md`
- Type: book chapter opening
- URL: https://doc.rust-lang.org/stable/book/ch10-00-generics.html
- Date processed: 2026-05-07

## Detailed Summary

Chapter 10 Rustdagi duplicationni kamaytirish vositalarini ochadi: [[generics]], [[traits]], va [[lifetimes]]. Har bir programming language concept duplicationni boshqarish uchun abstraction tools beradi; Rustda generics concrete type yoki boshqa property o'rnida abstract placeholder ishlatishga imkon beradi. Generic code compile/run paytida qaysi concrete type kelishini oldindan bilmasdan behavior yoki relationshipsni ifodalay oladi.

Generics function parameterlaridagi value placeholdersga o'xshaydi, lekin type darajasida ishlaydi. Function `i32` yoki `String` kabi concrete type o'rniga generic type parameter qabul qilishi mumkin. Oldingi chapterlarda generics allaqachon ishlatilgan: [[option|Option<T>]], [[vector|Vec<T>]], [[hash-map|HashMap<K, V>]], va [[result|Result<T, E>]]. Chapter 10 endi o'zimiz generic functions, structs, enums, va methods qanday define qilishni ko'rsatadi.

Chapter map uch qismdan iborat. Avval duplicated code'ni functionga extract qilish ko'rib chiqiladi. Keyin faqat parameter type'lari bilan farq qiladigan ikki functiondan generic function chiqarish o'rganiladi. So'ngra generic types struct va enum definitions ichida ishlatiladi.

Traits generic behaviorni belgilash uchun ishlatiladi. Generic type har qanday type bo'lib qolmasligi, balki ma'lum behaviorga ega type bo'lishi kerak bo'lsa, [[traits]] constraint sifatida qo'shiladi. Bu `trait bounds` mavzusiga tayyorlaydi.

Lifetimes ham genericsning bir turi sifatida frame qilinadi: ular references bir-biri bilan qanday bog'langanini compilerga aytadi. [[lifetimes]] borrowed values valid bo'lishini ko'proq holatlarda compiler tekshira olishi uchun kerakli relationship information beradi.

Source keyingi qismda [[code-duplication|code duplication]]ni function extraction orqali kamaytirishni ko'rsatadi. Dastlab `number_list` ichidan eng katta sonni topadigan code yoziladi. U `let mut largest = &number_list[0];` bilan birinchi elementga reference oladi, `for number in &number_list` orqali list bo'ylab yuradi, va `number > largest` bo'lsa `largest` reference'ini yangilaydi.

Ikkinchi list uchun bir xil logic copy-paste qilinsa, code ishlaydi, lekin duplication tedious va error-prone. Agar logic o'zgarsa, bir nechta joyda update qilish kerak. Bu holatda abstraction yaratilib, `largest(list: &[i32]) -> &i32` functioni chiqariladi. Function `&[i32]` slice qabul qiladi va list ichidagi largest elementga reference qaytaradi.

Extraction steps:

1. Duplicate code'ni identify qilish.
2. Duplicate code'ni function body'ga ko'chirish, inputs va return valuesni function signature'da ko'rsatish.
3. Duplicate code joylarini function call bilan almashtirish.

Bu steps keyingi generic function extraction uchun mental model bo'ladi. Function body concrete values o'rniga abstract `list` ustida ishlaganidek, generics code'ga abstract types ustida ishlash imkonini beradi.

## Key Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[traits]]
- [[lifetimes]]
- [[code-duplication|code duplication]]
- [[function-extraction|function extraction]]
- [[functions]]
- [[slices]]
- [[abstraction]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[option|Option<T>]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap<K, V>]]
- [[result|Result<T, E>]]

## Code Examples

Duplicated largest logic extracted into a function:

```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {result}");
    assert_eq!(*result, 100);

    let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let result = largest(&number_list);
    println!("The largest number is {result}");
    assert_eq!(*result, 6000);
}
```

Generic examples already seen:

```rust
Option<T>
Vec<T>
HashMap<K, V>
Result<T, E>
```

## Exercises or Practice Ideas

- Listing 10-2dagi duplicated largest logicni topib, qaysi lines function body bo'lishini belgilang.
- `largest(list: &[i32]) -> &i32` signature'ida input va return type nimani anglatishini yozing.
- `largest` functioni nega owned `Vec<i32>` emas, borrowed slice `&[i32]` qabul qilayotganini tushuntiring.
- `Option<T>`, `Vec<T>`, `HashMap<K, V>`, va `Result<T, E>`dagi generic parameterlar nimani bildiradi?
- `i32` list va `char` list uchun duplicated largest functions qanday qilib generic functionga aylanishi mumkinligini oldindan taxmin qiling.

## Questions Raised

- Generic function uchun type parameterga qaysi behavior kerakligini Rust qanday biladi?
- `largest` generic bo'lishi uchun `>` operation qaysi trait orqali constrain qilinadi?
- Lifetimes genericsning bir turi sifatida references relationshipni qanday ifodalaydi?
- Function extraction va generic abstraction orasidagi mental model o'xshashligi nimada?

## Links To Update

- [[10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[traits]]
- [[lifetimes]]
- [[code-duplication|code duplication]]
- [[function-extraction|function extraction]]
- [[largest-function-extraction|largest function extraction]]
- [[functions]]
- [[slices]]
