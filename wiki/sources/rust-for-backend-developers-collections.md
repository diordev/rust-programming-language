---
title: "Коллекции - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, collections, advance]
source_count: 1
---

# Коллекции - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/47. Коллекции.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/collections.html

## Detailed Summary

Bu source standard collections qatlamini Chapter 8 dagi basic `Vec`, `String`, `HashMap`dan kengaytiradi va backend kodda collection tanlash qarorini aniqroq qiladi. Kiritilgan to'plam: `Vec<T>`, `LinkedList<T>`, `VecDeque<T>`, `HashMap<K, V>`, `HashSet<T>`, `BTreeMap<K, V>`, `BinaryHeap<T>`.

`Vec` bo'limi capacity va oddiy mutatsiya API'larini qayta ko'rsatadi: `with_capacity`, `push`, `extend_from_slice`, `extend`, `sort`, `sort_by`, `pop`, `dedup`. Bu yerda muhim signal shuki, vector ko'p hollarda default collection bo'lib qoladi; boshqa collectionga o'tish faqat access/update pattern o'zgarganda oqlanadi.

`LinkedList` ikki tomonlama bog'langan ro'yxat sifatida tushuntiriladi. Source middle insert/delete'ni "effektiv" deb ataydi, lekin durable synthesis buni soddalashtirmaydi: list node'iga yetib borishning o'zi traversal talab qiladi. Demak middle insert/delete faqat position allaqachon topilgan yoki iteration cursor modelida turgan case'larda foydali; random indexing esa `O(n)`.

`VecDeque` ring buffer modeli bilan beriladi. U `push_front`, `push_back`, `pop_front`, `pop_back` uchun qulay, va index bo'yicha access vector kabi `O(1)` bo'lishi mumkin. Lekin "middle operationlar tez" degan umumlashma noto'g'ri: middle insert/remove hali ham element siljitishi mumkin. Asosiy yutug'i front/back queue workload'larida.

`HashMap` bo'limi hash table modelini va API'ni eslatadi: `insert`, `iter`, `keys`, `values`, `get`, `remove`, `contains_key`. Source ichida value uchun `PartialEq` talabi aytilgan, lekin durable correction: `HashMap`ning key side contracti `[[eq-trait|Eq]] + [[hash-trait|Hash]]`; oddiy `insert/get/remove` ishlashi uchun value type'dan `PartialEq` talab qilinmaydi.

`HashSet` `HashMap<T, ()>` ustidagi to'plam sifatida beriladi. U unique elementlar to'plami bo'lib, `insert`, `get`, `take`, `contains` kabi operationlarni beradi. Key insight: uniqueness va fast membership check kerak bo'lsa `HashSet`, payload bilan map kerak bo'lsa `HashMap`.

`BTreeMap` sorted map modeli. API `HashMap`ga juda o'xshaydi, lekin key contracti boshqa: `Hash` emas, `[[ord-trait|Ord]]` kerak. Shuning evaziga iteration sorted bo'ladi. Hash map bilan farq aynan ordering va comparison-based structure'da.

`BinaryHeap` priority queue sifatida ko'rsatiladi. `pop()` eng katta elementni qaytaradi, ya'ni standard implementation max-heap. `Ord` talabi shu yerda ham semantic jihatdan markaziy: heap priority'ni comparison belgilaydi.

Natijada source collection tanlashni performance claimlar bilan emas, operation pattern bilan o'qishni talab qiladi: random accessmi, front queue'mi, uniquenessmi, sorted order'mi, priority'mi.

## Key Concepts

- [[collections]]
- [[vector|Vec<T>]]
- [[linked-list|LinkedList]]
- [[vecdeque|VecDeque]]
- [[hash-map|HashMap]]
- [[hash-set|HashSet]]
- [[btree-map|BTreeMap]]
- [[binary-heap|BinaryHeap]]
- [[eq-trait|Eq]]
- [[hash-trait|Hash]]
- [[ord-trait|Ord]]

## Code Examples

```rust
let mut v: Vec<i32> = Vec::with_capacity(10);
v.extend_from_slice(&[1, 2, 3]);
v.sort();
```

```rust
use std::collections::VecDeque;

let mut q = VecDeque::from([1, 2, 3]);
q.push_front(0);
```

```rust
use std::collections::BinaryHeap;

let mut heap = BinaryHeap::new();
heap.push(9);
heap.push(2);
assert_eq!(heap.pop(), Some(9));
```

## Exercises or Practice Ideas

- Bir xil workload uchun `Vec`, `VecDeque`, va `LinkedList`dan qaysi biri mosligini yozma ravishda asoslang.
- `HashMap` va `BTreeMap` bilan bitta key-value dataset qurib, iteration order farqini ko'ring.
- `BinaryHeap` yordamida top-3 elementni olish misolini yozing.

## Questions Raised

- Default collection sifatida `Vec`dan qachon voz kechish kerak?
- Front/back queue pattern uchun `VecDeque` qachon real foyda beradi?
- Stable ordering kerak bo'lsa `BTreeMap` narxini qachon qabul qilish to'g'ri?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-collections]]
- [[collections]]
- [[hash-map|HashMap]]
- [[linked-list|LinkedList]]
- [[vecdeque|VecDeque]]
- [[hash-set|HashSet]]
- [[btree-map|BTreeMap]]
- [[binary-heap|BinaryHeap]]

