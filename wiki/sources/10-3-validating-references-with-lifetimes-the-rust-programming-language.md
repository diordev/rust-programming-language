---
title: "10.3. Validating References with Lifetimes"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, lifetimes, borrow-checker, references]
source_count: 1
---

# 10.3. Validating References with Lifetimes

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: Chapter 10.3
- URL: https://doc.rust-lang.org/stable/book/ch10-03-lifetime-syntax.html
- Raw fayl: `raw/books/the_rust_programming_language/10.3. Validating References with Lifetimes - The Rust Programming Language.md`

## Detailed Summary

### Lifetimes nima

[[lifetimes|Lifetimes]] genericsning bir turi. Typlar faqat kerakli behaviorga ega ekanini kafolatlasa, lifetimes references kerak bo'lgan vaqt davomida valid ekanini kafolatlaydi.

Har bir reference Rustda **lifetime**ga ega — bu reference valid bo'lgan scope. Ko'pincha lifetimes yashirin va inferred bo'ladi, xuddi typelar inferred bo'lgani kabi. Bir nechta references o'zaro munosabati noaniq bo'lganda esa annotatsiya talab qilinadi.

### Dangling Reference muammosi

Lifetimesning asosiy maqsadi — [[dangling-reference|dangling reference]]larni oldini olish. Dangling reference program o'z ma'lumotlari o'rniga boshqa ma'lumotlarga ishora qiladigan reference.

```rust
fn main() {
    let r;
    {
        let x = 5;
        r = &x;   // x bu ichki scope'da yaratildi
    }             // x bu yerda dropped bo'ladi
    println!("r: {r}");  // r endi danglingdir
}
```

Bu kod compile bo'lmaydi: `error[E0597]: x does not live long enough`.

### Borrow Checker

Rust compileri **borrow checker** deb ataluvchi mexanizmga ega. U scope'larni solishtiradi va barcha borrowlarning valid ekanini aniqlaydi.

```rust
fn main() {
    let r;                // ---------+-- 'a
                          //          |
    {                     //          |
        let x = 5;        // -+-- 'b  |
        r = &x;           //  |       |
    }                     // -+       |
                          //          |
    println!("r: {r}");   //          |
}                         // ---------+
```

`r` — `'a` lifetimega ega. `x` — `'b` lifetimega ega. `'b` `'a`dan kichik, shuning uchun program rad etiladi: reference subject, referencedan qisqaroq yashaydi.

To'g'ri variant — `x`ni tashqi scopeda e'lon qilish orqali `'b`ni `'a`dan katta qilish:

```rust
fn main() {
    let x = 5;            // ----------+-- 'b
    let r = &x;           // --+-- 'a  |
    println!("r: {r}");   //   |       |
                          // --+       |
}                         // ----------+
```

### Funksiyalarda Generic Lifetimes

`longest` funksiyasi ikkita string slice qabul qilib, uzunini qaytaradi:

```rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}
```

Bu compile bo'lmaydi: `error[E0106]: missing lifetime specifier`. Compiler return type qaysi parametrga bog'liqligini bilmaydi — `x`mi yoki `y`mi. Borrow checker ham bilmaydi, chunki `x` va `y` lifetimes orasidagi munosabat ko'rsatilmagan.

### Lifetime Annotation Sintaksisi

Lifetime annotatsiyalar references qancha yashashini o'zgartirmaydi — ular faqat bir nechta references o'zaro munosabatini ifodalaydi.

Sintaksis: `'` belgisi bilan boshlanadi, odatda lowercase va qisqa (`'a`, `'b`). `&` dan keyin yoziladi.

```rust
&i32        // oddiy reference
&'a i32     // explicit lifetime bilan reference
&'a mut i32 // explicit lifetime bilan mutable reference
```

Bitta annotatsiya yolg'iz ma'no bermaydi — ular bir nechta references munosabatini ifodalash uchun kerak.

### Funksiya Signaturasida Lifetime

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Bu signature shuni aytadi: `'a` lifetimesi uchun, funksiya ikkala parametrni `'a` dan kamida shuncha uzoq yashaydigan string slice sifatida qabul qiladi va shu lifetime bilan string slice qaytaradi.

Amalda: qaytariladigan reference `x` va `y`ning kichik lifetimesiga teng bo'ladi.

**Muhim:** Annotatsiyalar lifetime'ni o'zgartirmaydi, faqat borrow checker uchun constraint ifodalaydi.

```rust
// Bu ishlaydi — result ikala scope ichida ishlatilgan
fn main() {
    let string1 = String::from("long string is long");
    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {result}");
    }
}
```

```rust
// Bu compile bo'lmaydi — result string2 scope'dan tashqarida ishlatilgan
fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }
    println!("The longest string is {result}"); // E0597
}
```

### Munosabatlar (Relationships)

Agar funksiya har doim birinchi parametrni qaytarsa, `y` uchun lifetime kerak emas:

```rust
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
    x
}
```

Agar funksiya ichida yaratilgan local valuega reference qaytarmoqchi bo'lsak, bu dangling reference bo'ladi:

```rust
fn longest<'a>(x: &str, y: &str) -> &'a str {
    let result = String::from("really long string");
    result.as_str() // E0515: cannot return reference to local variable
}
```

Bunday holda owned type qaytarish kerak.

### Struct Definitionida Lifetime

Structlar reference fieldlar saqlashi mumkin, lekin har bir reference uchun lifetime annotatsiya kerak:

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().unwrap();
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

Bu `ImportantExcerpt` instance o'z `part` fielddagi referencedan uzoqroq yashay olmasligini bildiradi.

### Lifetime Elision

Ba'zi funksiyalar lifetime annotatsiyasiz compile bo'ladi:

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

Rust jamoasi Rust kodida qayta-qayta paydo bo'ladigan deterministic patternlarni aniqladi va ularni compiler ichiga kodladi. Bu patternlar **lifetime elision rules** deb ataladi.

**3 ta qoida:**

1. **Birinchi qoida** (input lifetimes uchun): Har bir reference parameter o'z lifetime parameterini oladi.
   - `fn foo(x: &i32)` → `fn foo<'a>(x: &'a i32)`
   - `fn foo(x: &i32, y: &i32)` → `fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`

2. **Ikkinchi qoida** (output lifetimes uchun): Agar bitta input lifetime parameter bo'lsa, u barcha output lifetime parametrlarga beriladi.
   - `fn foo<'a>(x: &'a i32) -> &'a i32`

3. **Uchinchi qoida** (output lifetimes uchun): Agar bir nechta input lifetime bo'lsa, lekin ulardan biri `&self` yoki `&mut self` bo'lsa (method bo'lsa), `self` lifetime barcha output lifetime parametrlariga beriladi.

`first_word` uchun: 1-qoida → `fn first_word<'a>(s: &'a str) -> &str`. 2-qoida → `fn first_word<'a>(s: &'a str) -> &'a str`. Tayyor.

`longest` uchun: 1-qoida → ikkita lifetime. 2-qoida — bir nechta input, qo'llanmaydi. 3-qoida — method emas, qo'llanmaydi. Natija: hali ham output lifetime aniqlanmagan → compiler error.

### Method Definitionida Lifetime

```rust
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}

impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {announcement}");
        self.part
    }
}
```

`announce_and_return_part`da: 1-qoida → `&self` va `announcement` har xil lifetime oladi. 3-qoida → `&self` bor, shuning uchun return type `self`ning lifetimini oladi. Annotatsiya kerak emas.

### `'static` Lifetime

`'static` butun dastur davomida yashashni bildiradi. Barcha string literallar `'static` lifetimega ega — ular dastur binary'si ichida saqlanadi:

```rust
let s: &'static str = "I have a static lifetime.";
```

**Ogohlantirish:** Compiler `'static` taklif qilganda darhol qo'llamang. Asosiy muammo — dangling reference yoki lifetime mismatch bo'lishi mumkin. Avval muammoning ildizini topping.

### Generics + Traits + Lifetimes birgalikda

```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {ann}");
    if x.len() > y.len() { x } else { y }
}
```

Lifetimes genericsning bir turi bo'lgani uchun `'a` va `T` bitta angle bracket ichida yonma-yon e'lon qilinadi.

## Key Concepts

- [[lifetimes]] — referencelar qancha vaqt valid ekanini ifodalovchi generic tur
- [[dangling-reference]] — scope'dan chiqqan valuega reference; Rust compile time'da rad qiladi
- [[borrow-checker]] — Rust compileri lifetime va borrow validationni bajaruvchi mexanizm
- [[lifetime-elision]] — compiler lifetimeni avtomatik infer qiladigan 3 ta qoida
- [[static-lifetime]] — `'static` — dastur davomida yashovchi lifetime
- [[e0597-does-not-live-long-enough]] — dangling reference yoki scope mismatch
- [[e0515-cannot-return-local-reference]] — funksiya ichidagi local valuega reference qaytarish
- [[e0106-missing-lifetime-specifier]] — reference qaytaruvchi funksiya/struct annotatsiya talab qiladi

## Code Examples

- **Dangling reference** — Listing 10-16, 10-17, 10-18
- **`longest` funksiyasi** — Listing 10-19, 10-20, 10-21, 10-22, 10-23
- **`ImportantExcerpt` struct** — Listing 10-24
- **`first_word` elision** — Listing 10-25
- **Kombinatsiya** — `longest_with_an_announcement`

## Exercises or Practice Ideas

- `longest` funksiyasiga turli scope va lifetimes bilan tajriba qiling: borrow checker qachon approve qiladi?
- `string2` va `result` deklaratsiyalarini turli scopelarga ko'chiring, compile natijasini bashorat qiling.
- `ImportantExcerpt`ni `novel` dan uzoqroq yashashga urinib ko'ring — xato xabarini o'rganing.
- `first_word`ga qo'l bilan 3 ta elision qoidasini bosqichma-bosqich qo'llang.
- `longest_with_an_announcement`ni `where T: Display + Debug` bilan kengaytiring.

## Questions Raised

- Advanced lifetime scenarios — masalan, lifetime subtyping va variance — qayerda o'rganiladi? (Rust Reference)
- Lifetime elision qoidalari kelajakda ko'payishi mumkinmi?
- Trait objects va lifetimes qanday birlashadi? (Chapter 18)

## Links To Update

- [[lifetimes]] — to'liq yangilash (borrow checker, elision, struct, `'static`)
- [[e0106-missing-lifetime-specifier]] — Chapter 10 kontekstini qo'shish
- [[index]] — yangi source qo'shish
- [[overview]] — Chapter 10 synthesisni yakunlash
