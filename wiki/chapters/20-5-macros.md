---
title: "20.5. Macros"
type: chapter
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, metaprogramming, advanced]
source_count: 1
---

# 20.5. Macros

## Learning Goal

Rust macro tizimining ikki katta oilasini tushunish: `macro_rules!` bilan [[declarative-macros|declarative macros]] va `TokenStream` asosidagi [[procedural-macros|procedural macros]]. Macro qachon functiondan farq qilishini, `vec!` macro qanday expand bo'lishini, va custom derive macro qanday crate structure talab qilishini bilish.

## Main Ideas

- [[macro|Macro]] - code yozadigan code, ya'ni [[metaprogramming]].
- Function runtime'da ishlaydi; macro compile-time'da expand bo'ladi.
- Macro variable number of arguments qabul qila oladi va trait implementation generatsiya qila oladi.
- `macro_rules!` declarative macro pattern/replacement modelida ishlaydi.
- Procedural macro `TokenStream -> TokenStream` transformatsiyasi.
- Procedural macro uch turi: [[custom-derive-macros|custom derive]], [[attribute-like-macros|attribute-like]], [[function-like-macros|function-like]].
- Procedural macro definitions alohida `proc-macro` crate ichida turadi.

## Bobning Tarkibi

| Bo'lim | Mavzu |
|--------|-------|
| Macro vs Function | Compile-time expansion, variable arguments, complexity |
| Declarative Macros | `macro_rules!`, `vec!` simplified definition |
| Procedural Macros | `TokenStream`, `proc-macro` crate |
| Custom derive | `#[derive(HelloMacro)]`, `syn`, `quote` |
| Attribute-like macros | `#[route(GET, "/")]` |
| Function-like macros | `sql!(...)` |

## Macro vs Function

| | Function | Macro |
|---|----------|-------|
| Ishlash vaqti | Runtime | Compile-time expansion |
| Argumentlar | Signatureda son/type aniq | Variable number va syntax patternlar |
| Trait impl generatsiya | Yo'q | Ha |
| O'qish/maintain qilish | Odatda sodda | Murakkabroq |
| Scope qoidasi | Keyin e'lon qilinsa ham chaqirish mumkin | Chaqirishdan oldin scope'da bo'lishi kerak |

## Asosiy Misollar

### Declarative `vec!`

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

Batafsil: [[simplified-vec-macro]].

### Procedural Macro Skeleton

```rust
use proc_macro::TokenStream;

#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    // parse tokens, generate tokens
}
```

### Custom derive

```rust
#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}
```

Batafsil: [[hello-macro-derive]].

### Attribute-like macro

```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
    // attr = GET, "/"
    // item = fn index() { ... }
}
```

### Function-like macro

```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    // input = SELECT * FROM posts WHERE id=1
}
```

Batafsil: [[procedural-macro-forms]].

## Concepts

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
- [[compiler]]
- [[crate]]
- [[dependencies]]

## Examples

- [[simplified-vec-macro|Simplified vec! macro definition]]
- [[hello-macro-derive|HelloMacro custom derive macro]]
- [[procedural-macro-forms|Attribute-like and function-like procedural macro forms]]

## Exercises

1. `my_vec!` macro yozing va `my_vec![1, 2, 3]`ni `Vec<i32>`ga expand qildiring.
2. `$x:expr`, `$()`, `,`, `*` macro pattern qismlarini misol bilan tushuntiring.
3. `HelloMacro` derive example uchun crate structure chizing: `hello_macro`, `hello_macro_derive`, `pancakes`.
4. `syn::parse(input)` nima qaytarishini `Pancakes` unit struct misolida tushuntiring.
5. `quote!` ichida `#name` va `stringify!(#name)` farqini yozing.
6. Attribute-like macro va function-like macro signature farqini yoddan yozing.
7. Macro yozish o'rniga function/generic/trait ishlatish yetarli bo'lgan 3 holat toping.

## Review Questions

1. Macro va function orasidagi 5 ta farq nima?
2. Macro nima uchun trait implementation generatsiya qila oladi, function esa yo'q?
3. `#[macro_export]` nima qiladi?
4. `( $( $x:expr ),* )` patternidagi har bir belgi nimani bildiradi?
5. `vec![1, 2, 3]` simplified expansion qanday ko'rinadi?
6. Procedural macro signature nima uchun `TokenStream -> TokenStream`?
7. `proc-macro = true` qayerda yoziladi?
8. `syn` va `quote` crate'lari qanday rol bajaradi?
9. `#[proc_macro_derive(HelloMacro)]` qaysi user-facing syntax bilan bog'lanadi?
10. Attribute-like macro va custom derive macro orasidagi asosiy farq nima?
11. Function-like macro oddiy functiondan nimasi bilan farq qiladi?

## Related Pages

- [[wiki/sources/20-5-macros|Source: 20.5]]
- [[wiki/chapters/20-4-advanced-functions-and-closures|Chapter 20.4: Advanced Functions and Closures]]
- [[macro]]
- [[vec-macro|vec! macro]]
- [[derive-attribute|derive attribute]]
- [[compiler]]
