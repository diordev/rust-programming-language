---
title: "Ownership, borrowing, reference va lifetimes bog'lanishi"
type: question
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, question, ownership, borrowing, reference, lifetimes]
source_count: 10
---

# Ownership, borrowing, reference va lifetimes bog'lanishi

## Answer

Bu to'rtta tushuncha Rustning bitta katta savoliga xizmat qiladi:

"Xotiradagi qiymat kimniki, undan kim foydalanishi mumkin, va bu foydalanish qachongacha xavfsiz?"

Eng qisqa mental model:

- [[ownership]] = qiymatning egasi kim?
- [[borrowing]] = egasidan vaqtincha foydalanish mumkinmi?
- [[reference]] = shu vaqtinchalik foydalanishning texnik ko'rinishi, ya'ni `&T` yoki `&mut T`
- [[lifetimes]] = reference qachongacha valid bo'lishini compiler qanday tekshiradi?

Maktabcha misol:

- daftar = heap yoki stack'dagi value
- egasi = owner
- do'stingizga daftarni berib turish = borrowing
- uning qo'lidagi "mana shu daftardan foydalan" yozuvi = reference
- "dars tugaguncha ishlatib tur" degan muddat = lifetime

Rustdagi katta g'oya shuki: qiymatning egasi aniq bo'ladi, vaqtinchalik foydalanish qoidalari aniq bo'ladi, va compiler bularni compile time'da tekshiradi. Shu sabab Rust garbage collector'siz ham [[memory-safety|memory safety]] beradi.

### 1. Ownership: kim egasi?

Ownership Rustning xotira boshqarish modeli. U ayniqsa heap data uchun muhim, masalan `String`, `Vec<T>`, `HashMap<K, V>`.

Asosiy 3 qoida:

- har bir value'ning owner'i bor
- bir vaqtda faqat bitta owner bo'ladi
- owner scope'dan chiqqanda value dropped bo'ladi

Misol:

```rust
let s1 = String::from("hello");
let s2 = s1;
// println!("{s1}"); // xato
```

Bu yerda `String` heapda saqlanadi. `s2 = s1` bo'lganda Rust heapdagi textni avtomatik chuqur copy qilmaydi. Uning o'rniga ownership `s1`dan `s2`ga o'tadi. Eski binding invalid bo'ladi. Bu [[move-semantics|move semantics]].

Nega bunday? Chunki aks holda `s1` ham, `s2` ham scope tugaganda bir xil heap memory'ni ikki marta tozalashga urinar edi. Rust shu xatoni oldindan to'xtatadi.

`Copy` type'lar boshqacha:

```rust
let x = 5;
let y = x;
println!("{x}, {y}");
```

`i32`, `bool`, `char` kabi kichik, fixed-size typelar [[copy-trait|Copy trait]] bilan ishlaydi. Ularda ownership move bo'lgandek ko'rinmaydi, chunki ular oddiy copy qilinadi.

Haqiqiy nusxa kerak bo'lsa:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
println!("{s1}, {s2}");
```

Bu yerda ownership ko'chmayapti, yangi heap nusxa yaralyapti.

### 2. Reference: vaqtinchalik qarash uchun ko'rsatkich

[[reference]] — bu value'ga ownership olmasdan ishora qiladigan qiymat. Syntax: `&value`.

Misol:

```rust
let s1 = String::from("hello");
let r = &s1;
println!("{r}");
```

Bu yerda `r` owner emas. U faqat `s1`ga qarab turibdi. Shuning uchun `r` yo'qolsa, `s1` drop bo'lmaydi. Faqat owner bo'lgan `s1` scope'dan chiqqanda cleanup bo'ladi.

Shu joydagi eng muhim farq:

- value = haqiqiy ma'lumot
- reference = shu ma'lumotga yo'l

Reference pointerga o'xshaydi, lekin Rustda u oddiy "xom adres" emas. Compiler reference noto'g'ri joyga qarab qolmasligini tekshiradi.

### 3. Borrowing: reference yaratish harakati

[[borrowing]] — reference yaratib, ownershipni olmasdan value'dan foydalanish.

Reference — narsa.
Borrowing — jarayon.

Misol:

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("{s1} -> {len}");
}
```

Bu yerda:

- `s1` owner
- `&s1` reference
- `calculate_length(&s1)` chaqirig'i borrowing

Function `String`ni ownership bilan olganida, caller keyin uni ishlata olmay qolardi. Borrowing esa ownershipni joyida qoldiradi.

Shu sabab quyidagicha o'ylash qulay:

- ownership = "buyum kimniki?"
- borrowing = "buyumni vaqtincha ishlatish mumkinmi?"

### 4. Immutable borrow va mutable borrow

Borrowing ikki xil bo'ladi:

- immutable borrow: `&T`
- mutable borrow: `&mut T`

Immutable borrow faqat o'qish uchun:

```rust
let s = String::from("hello");
let r1 = &s;
let r2 = &s;
println!("{r1} {r2}");
```

Bu ruxsat, chunki ikkisi ham faqat o'qiyapti.

Mutable borrow o'zgartirish uchun:

```rust
fn change(s: &mut String) {
    s.push_str(", world");
}

fn main() {
    let mut s = String::from("hello");
    change(&mut s);
    println!("{s}");
}
```

Bu yerda ikkita narsa birga kerak:

- original binding `mut` bo'lishi kerak
- function `&mut String` olishi kerak

Rustning qattiq qoidasi:

- yoki bitta mutable reference
- yoki istalgancha immutable references

Lekin ular bir vaqtning o'zida overlap qilmaydi.

Xato misol:

```rust
let mut s = String::from("hello");

let r1 = &s;
let r2 = &s;
let r3 = &mut s; // xato

println!("{r1} {r2} {r3}");
```

Nega xato? Chunki bir joy o'qib turganda, boshqa joy shu data'ni yozishga urinyapti. Rust buni [[e0502-mutable-and-immutable-borrow-conflict|E0502]] bilan to'xtatadi.

Yana bir xato:

```rust
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // xato
```

Bu [[e0499-multiple-mutable-borrows|E0499]]. Rust bir xil data'ni bir vaqtning o'zida ikki joydan mutate qilishga yo'l qo'ymaydi.

Bu qoidalar keyinchalik [[data-race|data race]]larni ham oldini oladi.

### 5. Ownership va borrowing birga qanday ishlaydi?

Rustni yaxshi tushunish uchun bu formulani eslab qolish foydali:

- owner = cleanup uchun javobgar
- borrower = vaqtincha foydalanuvchi
- reference = borrower qo'lidagi yo'l

Misol:

```rust
fn takes_ownership(s: String) {
    println!("{s}");
}

fn borrows(s: &String) {
    println!("{s}");
}

fn main() {
    let s = String::from("hello");

    borrows(&s);          // ownership ketmadi
    println!("{s}");      // ishlaydi

    takes_ownership(s);   // ownership ketdi
    // println!("{s}");   // endi ishlamaydi
}
```

Bu bitta misolda ownership va borrowing farqi juda aniq ko'rinadi.

### 6. Lifetimes: reference qachongacha xavfsiz?

Endi eng nozik qism: [[lifetimes]].

Reference owner emas. U faqat boshqa value'ga qaraydi. Demak compiler quyidagi savolni tekshirishi kerak:

"Reference ishlatilayotgan paytda, u qarayotgan value hali tirikmi?"

Agar yo'q bo'lsa, dangling reference bo'ladi.

Misol:

```rust
fn main() {
    let r;

    {
        let x = 5;
        r = &x;
    }

    println!("{r}"); // xato
}
```

Bu yerda `x` ichki scope tugashi bilan yo'q bo'ladi. `r` esa tashqarida ishlatilmoqchi. Demak `r` endi yo'q bo'lib ketgan narsaga qarab turadi. Rust buni compile qilmaydi.

Mana shu muammoni formal tekshiradigan tizim — lifetimes.

Muhim nuqta:

- lifetimes ma'lumotni uzoqroq yashatmaydi
- lifetimes faqat reference'lar orasidagi munosabatni compilerga tushuntiradi

Ya'ni `'a` yozish bilan value'ning umri cho'zilmaydi. Faqat "bu reference mana shu data bilan bog'langan" degan contract paydo bo'ladi.

### 7. Lifetime annotations qachon kerak bo'ladi?

Ko'p oddiy joylarda compiler o'zi tushunadi.

Masalan:

```rust
fn first_word(s: &str) -> &str {
    &s[..]
}
```

Bu yerda lifetime yozilmagan, lekin compiler bitta input reference va bitta output reference orasidagi bog'lanishni tushunadi. Bu [[lifetime-elision]] qoidalari sabab.

Lekin ba'zi holatlarda noaniqlik paydo bo'ladi:

```rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}
```

Bu nima uchun xato? Chunki return qilinayotgan reference `x`danmi yoki `y`danmi, compiler bilmaydi. Shuning uchun explicit lifetime yozamiz:

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Bu "ikkala argument ham aynan bir xil umr ko'radi" degani emas. Bu shuni bildiradi:

- return value lifetime'i `x` va `y` orasidagi overlap'dan kichigi bilan cheklanadi

Oddiyroq aytganda, `longest` qaytargan reference ikki argumentdan qisqaroq yashaganidan uzoqqa bora olmaydi.

### 8. Lifetimes bor, lekin reference yo'q joyda kerak emas

Owned type qaytarsangiz, lifetime muammosi ancha kamayadi:

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}
```

Bu xavfsiz, chunki function borrowed narsa qaytarmayapti. U yangi owner qaytaryapti.

Ammo bu xato:

```rust
fn bad() -> &String {
    let s = String::from("hello");
    &s
}
```

Sabab: function tugashi bilan `s` drop bo'ladi, reference esa tirik qolib ketmoqchi. Bu [[dangling-reference|dangling reference]].

### 9. Struct ichida reference bo'lsa, lifetime type'ning bir qismiga aylanadi

Misol:

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```

Bu yerda struct owned `String` saqlamayapti. U faqat boshqa joydagi matndan borrowed `&str` saqlayapti. Shuning uchun compilerga:

"Bu struct ichidagi reference qaysi muddatgacha valid?"

degan savolga javob kerak bo'ladi. Lifetime ana shu javobni type darajasida yozadi.

Shu sabab beginner bosqichida ko'p hollarda `String` saqlash `&str` saqlashdan soddaroq bo'ladi: owner aniq bo'ladi, lifetime contract kamroq bo'ladi.

### 10. To'rttasining bog'lanishi bitta oqimda

Mana to'liq zanjir:

1. Rustda har bir murakkab value'ning egasi bo'ladi. Bu [[ownership]].
2. Egasi bo'lmagan kod ham value'dan foydalana olishi kerak. Bu [[borrowing]].
3. Shu vaqtinchalik foydalanish `&T` yoki `&mut T` ko'rinishida bo'ladi. Bu [[reference]].
4. Reference noto'g'ri joyga qarab qolmasligi kerak. Compiler shu munosabatni [[lifetimes]] orqali tekshiradi.

Bir jumlada:

ownership value'ni boshqaradi, borrowing value'dan foydalanishni boshqaradi, reference shu foydalanishning sintaktik ko'rinishi, lifetime esa uning xavfsizligini tekshiradi.

### 11. Eng ko'p adashiladigan joylar

`reference` va `borrowing`ni bir xil deb o'ylash.
Yo'q. Reference — obyekt, borrowing — harakat va munosabat.

`mut` ownership beradi deb o'ylash.
Yo'q. `mut` faqat o'zgartirishga ruxsat. Egasi alohida tushuncha.

`'a` yozsam data uzoqroq yashaydi deb o'ylash.
Yo'q. Lifetime annotation hech narsani cho'zmaydi, faqat contractni ifodalaydi.

`&String` va `String` functionda bir xil deb o'ylash.
Yo'q. `String` ownershipni berishi mumkin, `&String` esa borrow.

Compiler `'static` desa, doim `'static` yozish kerak deb o'ylash.
Ko'pincha yo'q. Bu ko'pincha asl muammo reference juda qisqa umr ko'rayotganini bildiradi.

### 12. Juda qisqa yakun

Rustda bu mavzuni quyidagicha yodda saqlash oson:

- value kimniki? -> ownership
- undan vaqtincha foydalansa bo'ladimi? -> borrowing
- vaqtincha foydalanish nimada ifodalanadi? -> reference
- bu foydalanish qachongacha xavfsiz? -> lifetimes

Shu to'rttasi birga Rustning eng muhim kafolatini beradi: noto'g'ri xotira ishlatish holatlarini runtime'gacha olib bormaslik.

## Evidence

- [[ownership]]: owner, move, drop, va heap cleanup qoidalari.
- [[borrowing]]: ownershipni olmasdan foydalanish va aliasing/mutation qoidalari.
- [[reference]]: `&T` owner emasligi va faqat value'ga ishora qilishi.
- [[mutable-reference|mutable reference]]: `&mut T` va exclusive access qoidasi.
- [[lifetimes]]: reference validity va relationship contractlari.
- [[dangling-reference|dangling reference]]: invalid reference nima uchun rad qilinishi.
- [[lifetime-elision]]: compiler qachon explicit `'a`siz ham ishlay olishi.
- [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]]: ownership, borrowing, references, slices bo'yicha umumiy chapter synthesis.
- [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]]: lifetimes genericsning bir turi sifatida.
- [[4-1-what-is-ownership]]: ownership rules, `String`, move, `clone`, `Copy`.
- [[4-2-references-and-borrowing]]: immutable borrow, mutable borrow, borrow checker, dangling reference.
- [[10-3-validating-references-with-lifetimes]]: `longest`, lifetime annotations, elision, `ImportantExcerpt`.
- Raw checked: `raw/books/the_rust_programming_language/4.1. What is Ownership?.md`
- Raw checked: `raw/books/the_rust_programming_language/4.2. References and Borrowing.md`
- Raw checked: `raw/books/the_rust_programming_language/10.3. Validating References with Lifetimes.md`

## Follow-up

- `String`, `&String`, va `&str`ni alohida bir jadvalda solishtirish.
- Lifetime errorlarni (`E0106`, `E0597`, `E0515`) misolma-misol yechib chiqish.
- `Vec<T>`, `HashMap<K, V>`, va struct methodlarida ownership/borrowing qanday ishlashini mashq qilish.

## Related Pages

- [[ownership-haqida]]
- [[borrowing-nima-va-qanday-ishlaydi]]
- [[lifetimes-nima-va-nima-uchun-kerak]]
- [[generic-lifetime-qanday-ishlatiladi]]
- [[ownership]]
- [[borrowing]]
- [[reference]]
- [[mutable-reference|mutable reference]]
- [[lifetimes]]
- [[dangling-reference|dangling reference]]
- [[lifetime-elision]]
- [[move-semantics]]
- [[copy-trait]]
- [[scope]]
- [[slices]]
