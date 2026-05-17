---
title: "20.5. Macros"
type: source
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, metaprogramming, advanced]
source_count: 1
---

# 20.5. Macros

## Source

- File: `raw/books/the_rust_programming_language/20.5. Macros.md`
- URL: <https://doc.rust-lang.org/stable/book/ch20-05-macros.html>
- Book: *The Rust Programming Language*

## Detailed Summary

20.5 Rust'dagi [[macro|macros]] oilasini yakunlovchi advanced bo'lim. Macro - code yozadigan code, ya'ni [[metaprogramming|metaprogramming]] mexanizmi. Rust Book bu bo'limda macro'larni ikki katta oilaga ajratadi:

1. [[declarative-macros|Declarative macros]] - `macro_rules!`, "macros by example".
2. [[procedural-macros|Procedural macros]] - `TokenStream` qabul qilib, `TokenStream` qaytaradigan compile-time functionlar.

Procedural macros uch turga bo'linadi:

- [[custom-derive-macros|Custom derive macros]] - `#[derive(Name)]`.
- [[attribute-like-macros|Attribute-like macros]] - `#[route(GET, "/")]` kabi custom attributes.
- [[function-like-macros|Function-like macros]] - `sql!(...)` kabi function callga o'xshash macro call.

### Macro va Function Farqi

Function runtime'da chaqiriladi va signature parametrlar soni hamda typelarini aniq belgilaydi. Macro esa compile-time'da expand bo'lib, Rust code generatsiya qiladi.

Macro'larning functionlarga nisbatan qo'shimcha kuchlari:

- Variable number of arguments qabul qila oladi: `println!("hi")`, `println!("hi {}", name)`.
- Compile-time'da trait implementation generatsiya qila oladi.
- Rust syntaxining tokenlari ustida ishlaydi.
- Code duplicationni kamaytiradi.

Narxi:

- Macro definition functionga qaraganda murakkabroq.
- Macro "code yozadigan code" bo'lgani uchun o'qish va debug qilish qiyinroq.
- Macro call qilinishidan oldin definition scope'da bo'lishi kerak.

### Declarative Macros with `macro_rules!`

[[declarative-macros|Declarative macro]] pattern/replacement modelida ishlaydi. U `match`ga o'xshaydi, lekin value emas, Rust source code structure patternlariga qaraydi.

Simplified `vec!` macro:

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

Pattern `( $( $x:expr ),* )` quyidagilarni bildiradi:

- `$x:expr` - har qanday Rust expressionni `$x` nomi bilan capture qiladi.
- `$()` - repetition group.
- `,` - elementlar orasidagi literal comma separator.
- `*` - zero or more repetition.

`vec![1, 2, 3]` macro expansion natijasi soddalashtirilgan ko'rinishda:

```rust
{
    let mut temp_vec = Vec::new();
    temp_vec.push(1);
    temp_vec.push(2);
    temp_vec.push(3);
    temp_vec
}
```

Haqiqiy standard library `vec!` implementationi memoryni oldindan allocate qilish kabi optimizatsiyalar ham qiladi.

`#[macro_export]` macro crate scope'ga olib kirilganda export bo'lishini bildiradi.

### Procedural Macros

[[procedural-macros|Procedural macro]] pattern matching emas, functionga o'xshash compile-time transformatsiya:

```rust
use proc_macro::TokenStream;

#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {
}
```

Input `TokenStream` - macro qo'llangan code tokenlari. Output `TokenStream` - compile jarayoniga qo'shiladigan generated Rust code.

Procedural macro definitions hozircha alohida `proc-macro` crate ichida turishi kerak:

```toml
[lib]
proc-macro = true
```

Rust Book uch asosiy crate'ni ko'rsatadi:

- `proc_macro` - compiler API; Rust bilan birga keladi.
- `syn` - Rust code tokenlarini syntax tree'ga parse qiladi.
- `quote` - syntax tree yoki variablesdan Rust code tokenlari generatsiya qiladi.

### Custom `derive` Macro

Book `HelloMacro` traiti misolida `#[derive(HelloMacro)]` macro yaratishni ko'rsatadi. Maqsad: user har type uchun `impl HelloMacro` yozmasin; macro compile-time'da implementation generatsiya qilsin.

Foydalanuvchi yozadigan code:

```rust
use hello_macro::HelloMacro;
use hello_macro_derive::HelloMacro;

#[derive(HelloMacro)]
struct Pancakes;

fn main() {
    Pancakes::hello_macro();
}
```

Trait:

```rust
pub trait HelloMacro {
    fn hello_macro();
}
```

Derive macro crate convention:

- Main crate: `hello_macro`.
- Derive crate: `hello_macro_derive`.

`hello_macro_derive` crate:

```toml
[lib]
proc-macro = true

[dependencies]
syn = "2.0"
quote = "1.0"
```

Macro implementation:

```rust
use proc_macro::TokenStream;
use quote::quote;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let generated = quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}!", stringify!(#name));
            }
        }
    };
    generated.into()
}
```

Muhim nuqtalar:

- `syn::parse` `TokenStream`ni `DeriveInput` syntax tree'ga aylantiradi.
- `ast.ident` annotated type nomini beradi (`Pancakes`).
- `quote!` Rust code generatsiya qiladi.
- `#name` quote template ichida Rust variable qiymatini tokenlarga qo'yadi.
- `stringify!` compile-time'da tokenlarni string literalga aylantiradi.
- `unwrap` example uchun sodda; production macro yaxshiroq error messages berishi kerak.

Rust'da runtime reflection yo'q, shuning uchun type nomi asosida code generatsiya qilish compile-time macro orqali qilinadi.

### Attribute-like Macros

[[attribute-like-macros|Attribute-like macro]] custom attribute yaratadi. `derive` faqat structs/enums bilan ishlasa, attribute-like macro har xil itemlarga, masalan functionlarga qo'llanishi mumkin.

Example:

```rust
#[route(GET, "/")]
fn index() {
}
```

Definition signature:

```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
}
```

- `attr` - attribute ichidagi tokenlar: `GET, "/"`.
- `item` - attribute biriktirilgan item: `fn index() { ... }`.

### Function-like Macros

[[function-like-macros|Function-like macro]] function callga o'xshaydi, lekin tokenlar ustida compile-time transformatsiya qiladi.

Example:

```rust
let sql = sql!(SELECT * FROM posts WHERE id=1);
```

Definition:

```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
}
```

Bunday macro SQL syntaxini parse qilib, compile-time'da tekshirishi mumkin. Bu `macro_rules!` uchun murakkab bo'lgan domain-specific syntax processingga mos.

## Key Concepts

- [[macro|macros]]
- [[metaprogramming]]
- [[declarative-macros|declarative macros]]
- [[procedural-macros|procedural macros]]
- [[custom-derive-macros|custom derive macros]]
- [[attribute-like-macros|attribute-like macros]]
- [[function-like-macros|function-like macros]]
- [[tokenstream|TokenStream]]
- [[vec-macro|vec! macro]]
- [[derive-attribute|derive attribute]]
- [[traits]]
- [[compiler]]
- [[crate]]
- [[dependencies]]

## Code Examples

- [[simplified-vec-macro|Simplified vec! macro definition]] - Listing 20-35.
- [[hello-macro-derive|HelloMacro custom derive macro]] - Listings 20-37 through 20-42.
- [[procedural-macro-forms|Attribute-like and function-like procedural macro forms]] - `route` and `sql!` examples.

## Exercises or Practice Ideas

1. `my_vec!` declarative macro yozing: `my_vec![1, 2, 3]` `Vec<i32>` qaytarsin.
2. `macro_rules! say_hello` yozing: `say_hello!("Rust")` string format qilsin.
3. Simplified `vec!` macro patternidagi `$x:expr`, `$()`, `,`, `*` qismlarini alohida tushuntiring.
4. `HelloMacro` derive example'ini local workspace'da ikki crate bilan sinab ko'ring: `hello_macro` va `hello_macro_derive`.
5. `syn::DeriveInput`dan type nomini (`ident`) chiqarib, generated code ichida ishlating.
6. Attribute-like macro uchun `#[route(GET, "/")]` inputida `attr` va `item` tokenlari nimaga mos kelishini yozing.
7. Function-like macro `sql!(...)` nima uchun oddiy functiondan farq qilishini tushuntiring.

## Questions Raised

- Qachon macro yozish function/generic/traitdan yaxshiroq?
- Declarative macro va procedural macro orasidagi tanlov mezonlari nima?
- `syn` va `quote` production macro'larda qanday error handling bilan ishlatiladi?
- Macro expansion debug qilish uchun qaysi tool'lar (`cargo expand`) kerak?
- Custom derive macro main crate ichida re-export qilinsa, user dependency ergonomikasi qanday o'zgaradi?

## Links To Update

- [[index|Rust Wiki Index]] - 20.5 source/chapter, macro conceptlar, examples.
- [[overview]] - Chapter 20.5 synthesis.
- [[wiki/chapters/20-5-macros|Chapter 20.5]]
- [[macro]] - macro family overview.
- [[vec-macro]] - declarative macro internals.
- [[derive-attribute]] - custom derive macro context.
- [[crate]] - procedural macro crate convention.
