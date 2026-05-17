---
title: "Custom Derive Macros"
type: concept
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, macros, derive, proc-macro]
source_count: 1
---

# Custom Derive Macros

## Short Definition

`#[derive(Name)]` orqali struct yoki enum uchun compile-time code generation qiladigan procedural macro turi.

## Why It Matters

Custom derive repetitive trait implementationlarni avtomatik yaratadi. Rust runtime reflectionga tayanmaydi, shuning uchun type nomi yoki field structure asosida implementation kerak bo'lsa, macro compile-time'da code generatsiya qiladi.

## Mental Model

User:

```rust
#[derive(HelloMacro)]
struct Pancakes;
```

Macro expansion:

```rust
impl HelloMacro for Pancakes {
    fn hello_macro() {
        println!("Hello, Macro! My name is Pancakes!");
    }
}
```

## Syntax and Examples

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

Crate convention:

- Main trait crate: `hello_macro`.
- Derive macro crate: `hello_macro_derive`.

## Common Mistakes

- Trait va derive macro bir crate'da oddiy library sifatida turadi deb o'ylash; derive macro uchun `proc-macro` crate kerak.
- `derive` faqat structs/enumsga mosligini unutish.
- `syn::parse(input).unwrap()`ni production quality error handling deb qabul qilish.
- Generated code trait pathlarini user scope'iga bog'lab, noaniq qilish.

## Related Concepts

- [[derive-attribute|derive attribute]]
- [[procedural-macros|procedural macros]]
- [[tokenstream|TokenStream]]
- [[traits]]
- `quote!` macro from the `quote` crate
- [[hello-macro-derive]]

## Sources

- [[wiki/sources/20-5-macros|20.5 Macros]]
