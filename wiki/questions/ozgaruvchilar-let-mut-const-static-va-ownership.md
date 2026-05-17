---
title: "O'zgaruvchilar, let, let mut, const, static va ownership bog'lanishi"
type: question
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, question, variables, const, static, ownership]
source_count: 7
---

# O'zgaruvchilar, let, let mut, const, static va ownership bog'lanishi

## Answer

Rustda o'zgaruvchini oddiy "quti" deb emas, balki "nom + qiymat + qoidalar" deb tushunish kerak. `let`, `let mut`, `const`, va `static` nom yaratadi, lekin ularning qaysi biri qiymatni o'zgartirish, saqlash joyi, scope, va [[ownership]] bilan qanday bog'lanishini turlicha belgilaydi.

Eng sodda mental model:

- `let` = qiymatga yopishtirilgan ism, odatda o'zgarmaydi.
- `let mut` = qiymatga yopishtirilgan ism, keyin o'zgartirishga ruxsat bor.
- `const` = compile-time'da ma'lum bo'lgan doimiy qoida yoki raqam.
- `static` = dastur xotirasida bitta aniq, uzoq yashaydigan joy.
- `ownership` = "bu qiymat uchun kim javobgar va qachon tozalanadi?" degan qoida.

Maktab misoli bilan tasavvur qiling: sinfda daftarlar bor. Har bir daftar uchun bitta mas'ul o'quvchi bor. Bu ownership. Daftarga yopishtirilgan ism `let` bindingga o'xshaydi. Agar daftarni boshqa o'quvchiga bersangiz, endi eski o'quvchi u daftardan foydalana olmaydi. Bu move semantics.

```rust
let s1 = String::from("salom");
let s2 = s1;
// println!("{s1}"); // xato: ownership s2 ga o'tdi
```

Bu yerda `String` heap'da data saqlaydi. `s1`dan `s2`ga assignment bo'lganda Rust textni ikki marta ko'chirmaydi. U ownershipni `s2`ga o'tkazadi va `s1`ni invalid qiladi. Shuning uchun double free kabi memory buglar chiqmaydi.

`let` local binding yaratadi:

```rust
let yosh = 12;
```

Rustda `let` bilan yaratilgan variable default [[immutability|immutable]]. Demak:

```rust
let x = 5;
// x = 6; // E0384: immutable variable'ga qayta assignment
```

Bu "qiymat hech qachon o'zgarmaydi" degani emas, balki "shu binding orqali qayta assignment qilinmaydi" degani. Rust shu orqali code'ni o'qishni osonlashtiradi: agar `mut` ko'rinmasa, bu nom keyin o'zgarmaydi deb ishonsa bo'ladi.

Qiymatni keyin o'zgartirish kerak bo'lsa, `let mut` yoziladi:

```rust
let mut ball = 0;
ball = ball + 1;
```

`mut` ikki narsani bildiradi:

- compilerga: bu bindingni o'zgartirishga ruxsat ber;
- odamga: bu qiymat keyin o'zgaradi.

Owned data ichini o'zgartirishda ham `mut` kerak bo'ladi:

```rust
let mut name = String::from("Rust");
name.push_str(" tili");
```

Lekin `mut` ownershipni bekor qilmaydi. Quyidagi code'da `s1` mutable bo'lishi move'ni to'xtatmaydi:

```rust
let mut s1 = String::from("salom");
s1.push_str("!");

let s2 = s1; // ownership s2 ga o'tadi
// println!("{s1}"); // baribir xato
```

Demak, `mut` = "o'zgartirish mumkin", ownership esa = "kim egasi?". Ular bir-biriga bog'liq, lekin bir xil narsa emas.

`Copy` type'larda assignment ownership move qilmaydi, oddiy copy qiladi:

```rust
let x = 5;
let y = x;
println!("{x}, {y}");
```

`i32`, `bool`, `char` kabi kichik stack typelar `Copy`. `String` esa `Copy` emas, chunki u heap memoryga egalik qiladi. Agar haqiqiy nusxa kerak bo'lsa, explicit `clone()` yoziladi:

```rust
let s1 = String::from("salom");
let s2 = s1.clone();
println!("{s1}, {s2}");
```

Function chaqirish ham assignmentga o'xshaydi:

```rust
fn print_name(name: String) {
    println!("{name}");
}

let name = String::from("Ali");
print_name(name);
// println!("{name}"); // xato: ownership functionga o'tdi
```

Agar function faqat o'qishi kerak bo'lsa, ownershipni bermaymiz, borrow qilamiz:

```rust
fn uzunlik(s: &String) -> usize {
    s.len()
}

let name = String::from("Ali");
let n = uzunlik(&name);
println!("{name} uzunligi: {n}");
```

`&name` borrowing: "men bu qiymatni vaqtincha ko'rib turaman, egasi bo'lmayman." Shuning uchun `name` keyin ham valid.

Mutation kerak bo'lsa, mutable borrow kerak:

```rust
fn qosh(s: &mut String) {
    s.push_str("!");
}

let mut text = String::from("salom");
qosh(&mut text);
```

Bu yerda ikki qavatli ruxsat bor:

- original binding `let mut text` bo'lishi kerak;
- function `&mut String` qabul qilishi kerak.

Borrowing qoidasi: bir vaqtda yo bitta mutable reference, yoki ko'p immutable reference. Ikkalasi aralashib ketmasligi kerak. Bu Rustning data race va noto'g'ri o'zgarishlarni oldini olish usuli.

`const` local variable emas. U compile-time'da ma'lum bo'lgan, nomlangan doimiy qiymat:

```rust
const MAX_BALL: u32 = 100;
```

`const`:

- har doim immutable;
- `mut` olmaydi;
- type annotation talab qiladi;
- runtime hisoblangan qiymat emas, constant expression bo'lishi kerak;
- odatda `UPPER_SNAKE_CASE` bilan yoziladi.

`const`ni "maktabdagi qoida yozilgan plakat" deb o'ylang: "Dars 45 daqiqa", "Maksimal ball 100". Bu har bir o'quvchining shaxsiy daftari emas, hamma code uchun nomlangan qoida.

`static` esa `const`dan boshqacharoq. U dastur xotirasida bitta aniq joyga ega:

```rust
static APP_NAME: &str = "rust-wiki";
```

`const` = "qiymat ishlatilgan joyga qo'yib yuborilishi mumkin". `static` = "mana shu bitta global joy bor". Shuning uchun `static` address identity yoki shared state kerak bo'lgan paytda muhim.

`static` oddiy holatda immutable. Mutable global state kerak bo'lsa, `static mut` mavjud, lekin u `unsafe` talab qiladi:

```rust
static mut COUNTER: u32 = 0;

unsafe {
    COUNTER += 1;
}
```

Boshlovchi uchun qoida: `static mut`dan qoching. Global o'zgaruvchi kerak bo'lsa, ko'pincha `AtomicUsize`, `Mutex`, `RwLock`, `OnceLock`, yoki `LazyLock` kabi safe vositalar yaxshiroq.

`const` va `static` ownership bilan qanday bog'lanadi?

- `const` odatda ownership masalasini yengil qiladi, chunki u "nomlangan doimiy qiymat" sifatida ishlatiladi.
- `static` butun program davomida yashaydi, shuning uchun u bilan ko'pincha `'static` lifetime bog'liq bo'ladi.
- Lekin `static` keyword va `'static` lifetime bir xil emas.
- `static` global shared storage bo'lgani uchun mutation qilinsa, borrowing va concurrency qoidalari yanada muhimlashadi.

Kichik qaror daraxti:

- Qiymat shu scope ichida bir marta kerak va o'zgarmaydi: `let`
- Qiymat shu scope ichida o'zgaradi: `let mut`
- Bir xil nom bilan yangi bosqich yoki yangi type kerak: [[shadowing]]
- Compile-time doimiy limit yoki qoida kerak: `const`
- Dastur davomida bitta shared memory joyi kerak: `static`
- Functionga qiymatni butunlay berasiz: ownership move
- Function faqat o'qisin: `&T`
- Function o'zgartirsin, lekin egasi bo'lmasin: `&mut T`

Birlashtirilgan misol:

```rust
const MAX_LEN: usize = 20;
static APP_NAME: &str = "demo";

fn add_suffix(text: &mut String) {
    text.push_str("!");
}

fn main() {
    let app = APP_NAME; // &'static str, ownership olinmaydi

    let mut name = String::from("Rust");
    add_suffix(&mut name); // mutable borrow, ownership o'tmadi

    let len = name.len(); // Copy type: usize

    if len <= MAX_LEN {
        println!("{app}: {name}");
    }

    let moved = name; // String ownership moved ga o'tdi
    // println!("{name}"); // endi xato
    println!("{moved}");
}
```

Bu misolda hamma narsa birga ko'rinadi:

- `MAX_LEN` compile-time constant.
- `APP_NAME` butun dastur davomida yashaydigan static string.
- `name` mutable owned `String`.
- `add_suffix(&mut name)` ownershipni olmaydi, faqat vaqtincha mutable borrow qiladi.
- `len` `usize`, ya'ni `Copy`.
- `let moved = name` ownershipni ko'chiradi.

Eng qisqa xulosa: Rustda variable mavzusi faqat nom berish emas. Har bir nomning scope'i bor, mutability qoidasi bor, type'i bor, va ba'zi typelarda ownership bor. `let` va `let mut` local ishlar uchun, `const` compile-time doimiy qoidalar uchun, `static` global shared storage uchun. Ownership esa bularning ostidagi katta savolga javob beradi: "qiymat kimniki va u qachon tugaydi?"

## Evidence

- [[variables-and-mutability|variables and mutability]]: Rust variables default immutable; `mut` o'zgarish intentini bildiradi.
- [[constants]]: `const` doim immutable, typed, compile-time value.
- [[static-items]]: `static` bitta allocationga ega shared storage beradi.
- [[ownership]]: har bir value ownerga ega; owner scope'dan chiqqanda value dropped bo'ladi.
- [[move-semantics|move semantics]]: `String` kabi heap-backed value assignmentda ownershipni ko'chiradi.
- [[borrowing]]: `&T` va `&mut T` ownershipni bermasdan foydalanish imkonini beradi.
- [[copy-trait|Copy trait]]: kichik stack typelar assignmentda move emas, copy bo'ladi.
- [[3-1-variables-and-mutability]]: `let`, `mut`, `const`, va [[shadowing]] bo'yicha asosiy source summary.
- [[4-1-what-is-ownership]]: ownership rules, `String` move, `clone()`, va `Copy` farqi.
- [[4-2-references-and-borrowing]]: `&T`, `&mut T`, borrowing restrictionlari.
- [[const-va-static-farqi]]: `const` va `static` farqi bo'yicha saqlangan savol-javob.

## Follow-up

- `String` va `&str` ownership nuqtayi nazaridan alohida solishtirish.
- `static`, `'static lifetime`, va string literal farqini misollar bilan mashq qilish.
- `const fn`, `OnceLock`, va `LazyLock` qachon kerak bo'lishini o'rganish.

## Related Pages

- [[ozgaruvchilar-haqida]]
- [[const-va-static-farqi]]
- [[ownership-haqida]]
- [[variables-and-mutability]]
- [[constants]]
- [[static-items]]
- [[ownership]]
- [[borrowing]]
- [[reference]]
- [[mutable-reference]]
- [[move-semantics]]
- [[copy-trait]]
- [[scope]]
- [[shadowing]]
