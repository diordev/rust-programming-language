---
title: "Итераторы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, iterators]
source_count: 1
---

# Итераторы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/36. Итераторы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/iterators.html

## Detailed Summary

Bu source `for` loop'ni syntax convenience sifatida emas, `Iterator` va `IntoIterator` ustiga qurilgan abstraction sifatida tushuntiradi. Birinchi durable signal: `for` o'zi collection traversal bilmaydi; u iterator bilan ishlaydi. `Iterator` trait'ning markaziy nuqtasi `next(&mut self) -> Option<Self::Item>`. Shuning uchun iterator mental modeli "sequence ustidan manual cursor"ga yaqin: har chaqiriqda keyingi item yoki `None`.

Source custom `MyVec<T>` va `MyVecIter<'a, T>` orqali iteratorni qo'lda yozadi. Bu yerda muhim nuqta iterator odatda collectionning o'zi emas, collection bilan bog'langan alohida type bo'lishi. `current_ind` yoki `index` saqlanishi iteratorning asl state ekanini ko'rsatadi. `iter()` methodi trait tomonidan majburiy emas, lekin ecosystemdagi de-facto ergonomik API.

`for` va collection orasidagi haqiqiy bridge `IntoIterator`. Source aynan shu boundary'ni tozalaydi: `iter()` iteratorni explicit qaytaradi, `for x in &my_vec` esa `IntoIterator for &MyVec<T>` orqali implicit iterator yaratadi. Durable takeaway: `for x in collection` va `for x in &collection` ownership jihatdan bir xil emas. Reference orqali yurilsa borrowed itemlar olinadi; owned collection uzatilsa collection consume bo'lishi mumkin.

Source'da value-iterator misolida bitta xato bor: `Vec::pop()` "birinchi element" deb izohlanadi. Bu noto'g'ri; `pop()` oxirgi elementni oladi. Demak example ishlasa ham traversal order teskari chiqishi mumkin. Wiki'da bu caveat albatta saqlanadi, chunki bu ownership bo'yicha to'g'ri, tartib bo'yicha esa chalg'ituvchi misol.

Keyingi bo'lim standart `Vec` va array iteration usullarini bitta jadvalsiz, lekin aniq kontrast bilan beradi: `.iter()` va `&v` `&T` beradi, `.into_iter()` va `v` esa `T` berib collectionni consume qiladi. Bu source `IntoIterator`ni nazariy trait emas, everyday ownership switch sifatida foydali qiladi.

`Range` bo'limi yana bir foydali istisnoni ko'rsatadi. Oldin iterator ko'pincha alohida type deb aytilgan bo'lsa, `Range` o'zi `Iterator` bo'lib chiqadi. `0..20` faqat syntax emas; `std::ops::Range` value'si bo'lib, `next()` bilan to'g'ridan-to'g'ri ishlatilishi ham mumkin.

Source'ning eng muhim amaliy qismi iterator API. `filter` va `map` iteratorni darhol hisoblamaydi; ular yangi wrapper iteratorlar yaratadi. `collect()`gacha hech narsa "yakuniy" bajarilmaydi. Bu yerda lazy evaluation alohida yozilishi kerak: adapter chain consuming call bo'lmaguncha faqat computation plan yig'iladi. `collect::<Vec<_>>()` shu plan'ni ishga tushiradi.

`collect` bilan birga source universal iterator API g'oyasini beradi: ko'p collectionlar iterator beradi, ko'pi iterator'dan qayta quriladi. Bu Rust'da collection'lar ustidan transformation uchun bitta umumiy surface mavjudligini ko'rsatadi.

`fold` va `reduce` farqi source'da to'g'ri ajratilgan. `fold` tashqi initial accumulator qabul qiladi, shuning uchun result type item type'dan boshqacha bo'lishi mumkin. `reduce` esa first item'dan boshlaydi va collection bo'sh bo'lishi mumkinligi sabab `Option` qaytaradi. Bu distinction functional jargon emas; API tanlash mezoni.

Source numeric iterator specialization sifatida `sum()`ni ham eslatadi. Bu "Iterator trait ichida hamma narsa bir xil emas" signalini beradi: concrete trait bound'lar bo'lsa, qo'shimcha helperlar paydo bo'ladi.

Oxirgi amaliy metodlar `filter_map` va `find`. `filter_map` `Option`-producing transformni filter bilan birlashtiradi; `Some(v)` pipeline'ga o'tadi, `None` tashlab yuboriladi. `find` esa predicate'ga mos first item'ni topib `Option` qaytaradi. Bu ikkalasi iteratorlar va `Option` orasidagi bevosita bog'lanishni ko'rsatadi.

Source yakunida `itertools` crate tilga olinadi. Durable xulosa: standard `Iterator` juda kuchli, lekin ecosystem bu surface'ni kengaytiradi; iterator mental modeli bir crate bilan tugamaydi.

## Key Concepts

- [[iterators]]
- [[into-iterator|IntoIterator]]
- [[iterator-adapters|iterator adapters]]
- [[consuming-adapters|consuming adapters]]
- [[collect]]
- [[range]]
- [[for-loop|for loop]]
- [[option|Option]]
- [[closures]]
- [[fn-traits|Fn traits]]

## Code Examples

```rust
struct MyVecIter<'a, T> {
    data: &'a Vec<T>,
    index: usize,
}

impl<'a, T> Iterator for MyVecIter<'a, T> {
    type Item = &'a T;

    fn next(&mut self) -> Option<Self::Item> {
        if self.index < self.data.len() {
            let item = &self.data[self.index];
            self.index += 1;
            Some(item)
        } else {
            None
        }
    }
}
```

```rust
let v = vec![1, 2, 3];

for x in &v {
    println!("{x}");
}

for x in v.into_iter() {
    println!("{x}");
}
```

```rust
let v2 = vec![1, 2, 3, 4, 5]
    .into_iter()
    .filter(|x| x % 2 == 0)
    .map(|x| x * x)
    .collect::<Vec<_>>();
```

```rust
let sum = [1, 2, 3].into_iter().fold(0, |acc, x| acc + x);
let first_even = [1, 3, 5, 8].into_iter().find(|x| x % 2 == 0);
```

## Exercises or Practice Ideas

- `for x in v`, `for x in &v`, va `for x in v.iter()` uchun item type va ownership natijasini bir xil vector ustida yozib chiqing.
- `Iterator`ni qo'lda implement qiladigan kichik `Counter` yoki `RangeLike` type yozing.
- `map(...).collect()` va `filter_map(...)` bilan bir xil data-flow'ni ikki xil usulda yozib, qaysi biri signal jihatdan aniqroq ekanini solishtiring.
- `fold` va `reduce` bilan bir xil aggregation'ni yozib, bo'sh collection holatida farqni kuzating.
- `Vec::pop()`dan foydalangan value-iterator yozib, order nega teskari chiqishini ko'rsating.

## Questions Raised

- Qachon `iter()` bilan borrow qilish kerak, qachon `into_iter()` bilan ownershipni berish to'g'ri?
- `filter_map` qachon `map + flatten` yoki `filter + map`dan aniqroq signal beradi?
- `collect()` o'rniga `fold`, `sum`, yoki `find` qachon yaxshiroq terminal operation bo'ladi?
- Iterator laziness compile-time optimizationmi, runtime behavior modelimi, yoki ikkalasi hammi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-iterators]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[iterators]]
- [[into-iterator|IntoIterator]]
- [[iterator-adapters|iterator adapters]]
- [[consuming-adapters|consuming adapters]]
- [[collect]]
- [[range]]
- [[for-loop|for loop]]
- [[option|Option]]
- [[closures]]
- [[fn-traits|Fn traits]]
