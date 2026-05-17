---
title: "Основные трейты - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, traits, advance]
source_count: 1
---

# Основные трейты - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/45. Основные трейты.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/common-traits.html

## Detailed Summary

Bu source Rust standard/common traitlar qatlamini `3. advance` sectionining kirish nuqtasi sifatida beradi. Muhim farq shuki, oldingi `traits` source behavior abstraction, static/dynamic dispatch, va trait objectlarni tushuntirgan edi; bu source esa aynan standard library trait ecosystem'ining amaliy contractlarini ajratadi.

Kirish qismida oldin ko'rilgan traitlar sanaladi: `Debug`, `Clone`, `Copy`, `Hash`, `PartialEq`, `Eq`, `Default`, `Deref`, `Iterator`. So'ng tez-tez ishlatiladigan keyingi traitlar kiritiladi: `PartialOrd`, `Ord`, `From`/`Into`, `AsRef`, `Borrow`, `ToOwned`, `Drop`, `Sized`, `Sync`, `Send`.

`Eq` va `PartialEq` bo'limi equality semantics'ining nozik joyini ochadi. Durable takeaway: `PartialEq` tenglik operatsiyasini beradi, `Eq` esa shu tenglik to'liq reflexive contractga ega ekanini marker sifatida va'da qiladi. `NaN != NaN` misoli sabab `f32`/`f64` `PartialEq`, lekin `Eq` emas.

`PartialOrd` va `Ord` bo'limi order contractni ikki qatlamga ajratadi. `partial_cmp()` `Option<Ordering>` qaytaradi, chunki ayrim qiymatlar taqqoslanmasligi mumkin; `cmp()` esa har doim `Ordering` qaytaradi, ya'ni total order talab qiladi. `f32::NAN.partial_cmp(...) == None` shu boundaryni aniq ko'rsatadi.

Cargo example'i emas, `Cargo` struct misoli bilan source yana bitta muhim qoida beradi: agar type uchun `Ord` implement qilinsa, `PartialOrd` odatda shunchaki `Some(self.cmp(other))` orqali delegatsiya qilinadi. Ya'ni ikki alohida order logic yozish default yo'l emas.

`Default` bo'limi traitni "reasonable starting value" contracti sifatida ko'rsatadi. Standard typelar uchun `default()` misollari va `Option::unwrap_or_default()` practical signal beradi. Derive va manual impl orasidagi farq ham aniq: derive field-by-field default, manual impl esa domain-specific default bera oladi.

`From` va `Into` bo'limi conversion pair'ni amaliy ishlatishga olib keladi. `From<[u8; 4]> for Ip4Addr` explicit constructor sifatida tabiiy. `Into<Ip4Addr>`ni function argumentida qabul qilish ergonomik pattern. Lekin source'dagi eng muhim nuance: `Into`ni qo'lda implement qilish kamdan-kam kerak, chunki `From<X> for Y` avtomatik ravishda `Into<Y> for X`ni beradi.

`AsRef` va `Borrow` bo'limlari tashqaridan o'xshash ko'rinsa ham, contract jihatdan boshqa. `AsRef<T>` reference conversion: ichki data'ga `&T` ko'rinishida chiqish. `Borrow<T>` esa bundan kuchliroq: owner va borrowed view `Eq`/`Ord`/`Hash` semantics jihatdan mos bo'lishi kerak. Shu sabab `Borrow` oddiy `AsRef` replacement emas.

`AsMut` va `BorrowMut` mutable analoglar sifatida tilga olinadi. Ular source'da chuqur ochilmaydi, lekin `AsRef`/`Borrow` patternining mut bor variantlari ekanini qayd etish kerak.

`ToOwned` bo'limi borrowed value'dan owned value olishni ko'rsatadi. Bu ko'p holatda `Clone`ga o'xshaydi, lekin har doim bir xil type qaytarmaydi. `&str -> String` misoli aynan shu sabab bilan muhim.

`Drop` bo'limi destructor hook'ini ko'rsatadi. `MyBox<T>` misolida raw allocation ishlatiladi, lekin source o'zi ham bu production heap management namunasi emasligini ogohlantiradi. Durable takeaway boshqa: `Drop` scope exit paytida cleanup hook, unsafe allocator demo esa faqat mexanizmni ko'rsatish uchun.

`Sized` bo'limi compile-time size mental modelini aniq qilib beradi: `str`, `[T]`, `dyn Trait` o'zi `!Sized`; `&str`, `&[T]`, `Box<dyn Trait>` esa pointer/owner wrapper bo'lgani uchun `Sized`. Shuningdek genericlarda default `T: Sized` bound borligi va `?Sized` bilan shu bound bo'shatilishi ham muhim qism.

`Send` va `Sync` bo'limlari bu source'da faqat preview sifatida beriladi. Muhim signal: ular compiler automatic marker traitlari, lekin semantic ma'nosi thread safety contracti bilan bog'liq va keyinroq multithreading bobida chuqurlashadi.

## Key Concepts

- [[eq-trait]]
- [[partial-eq]]
- [[partial-ord]]
- [[ord-trait|Ord]]
- [[ordering|Ordering]]
- [[default-trait]]
- [[from-trait]]
- [[into-trait]]
- [[asref-trait]]
- [[asmut-trait]]
- [[borrow-trait]]
- [[borrowmut-trait]]
- [[to-owned-trait]]
- [[drop]]
- [[sized-trait]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]

## Code Examples

```rust
let a = f32::NAN;
let b = f32::NAN;
println!("{}", a == b); // false
```

```rust
println!("{:?}", 4.0.partial_cmp(&5.0));           // Some(Less)
println!("{:?}", f32::NAN.partial_cmp(&f32::NAN)); // None
```

```rust
impl From<[u8; 4]> for Ip4Addr {
    fn from(value: [u8; 4]) -> Self {
        let [a, b, c, d] = value;
        Ip4Addr(a, b, c, d)
    }
}
```

```rust
let slice: &str = "aaa";
let owned: String = slice.to_owned();
```

## Exercises or Practice Ideas

- `Eq` va `PartialEq` uchun `f32::NAN` misolini qayta yozib, nega `Eq` implement bo'lmasligini izohlang.
- Custom type uchun `Ord` yozib, `PartialOrd`ni `Some(self.cmp(other))` bilan delegatsiya qiling.
- `From<[u8; 4]> for Ip4Addr` yozing va keyin `impl Into<Ip4Addr>`ni alohida yozmasdan `into()` ishlashini tekshiring.
- `AsRef<Path>` va `Borrow<str>`ga o'xshash ikki API yozib, qaysi contract kuchliroq ekanini yozing.
- `T: ?Sized` qabul qiladigan bitta helper function yozib, `&str` va `&dyn Trait` bilan ishlating.

## Questions Raised

- Qachon `Eq` derive qilish safe, qachon domainda equality semantics alohida ko'rib chiqilishi kerak?
- `Borrow` contracti `AsRef`ga qaraganda kuchliroq bo'lsa, noto'g'ri `Borrow` impl qaysi collectionlarda xavfli bo'ladi?
- `From` implement qilib ham `Into`ni qo'lda yozish qachon kerak bo'lishi mumkin?
- `Sized` default boundni qachon ongli ravishda `?Sized`ga bo'shatish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-common-traits]]
- [[eq-trait]]
- [[partial-eq]]
- [[partial-ord]]
- [[ord-trait|Ord]]
- [[from-trait]]
- [[sized-trait]]
- [[borrow-trait]]
- [[to-owned-trait]]

