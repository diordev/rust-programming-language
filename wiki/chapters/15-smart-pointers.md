---
title: "15. Smart Pointers"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, memory, ownership]
source_count: 7
---

# 15. Smart Pointers

## Learning Goal

Smart pointerlar Rust memory modelini qanday kengaytirishini tushunish: pointer-like behavior, heap allocation, recursive types, `Deref`, `DerefMut`, deref coercions, `Drop`, reference counting, va interior mutability.

## Main Ideas

- [[reference|References]] data'ni borrow qiladi; ko'p [[smart-pointers|smart pointers]] esa data'ga ownership qiladi.
- [[box-t|Box<T>]] heap allocation va indirection beradi, lekin extra runtime bookkeeping deyarli qo'shmaydi.
- [[recursive-types|Recursive types]] compiler uchun finite size talab qiladi; `Box<T>` recursionni pointer orqali uzadi.
- [[deref-trait|Deref trait]] `*` operator behaviorini custom qiladi va smart pointerni reference kabi ishlatishga imkon beradi.
- [[deref-coercions|Deref coercions]] function/method calllarda `&T`ni kerakli reference typega compile time'da avtomatik moslaydi.
- [[deref-mut-trait|DerefMut trait]] mutable reference coercion uchun ishlatiladi.
- [[drop|Drop trait]] scope'dan chiqishda cleanup kodi ishlatadi; `Drop::drop` qo'lda chaqirilmaydi.
- [[rc-t|Rc<T>]] reference counting orqali bir qiymatga bir nechta egani ta'minlaydi (single-threaded).
- [[refcell-t|RefCell<T>]] borrow qoidalarini runtime da tekshiradi; [[interior-mutability|interior mutability]] patternini amalga oshiradi.
- `Rc<RefCell<T>>` kombinatsiyasi — ko'p egalik **va** mutable access beradi.

## Concepts

### 15.0 Smart Pointers Overview

Smart pointer pointer kabi ishlaydi, lekin oddiy addressdan tashqari metadata yoki behaviorga ega. Rust standard library smart pointerlari ownership, borrowing, cleanup, va runtime checking kabi masalalarni type orqali ifodalaydi.

Chapter 15 boshlanishi uchta keyingi type'ni belgilaydi: `Box<T>`, `Rc<T>`, va `RefCell<T>` orqali olinadigan `Ref<T>` / `RefMut<T>`. Hozirgi ingest scope 15.0, 15.1, 15.2 bilan cheklangan; `Rc<T>`, `RefCell<T>`, interior mutability va reference cycles keyingi source'larda to'ldiriladi.

### 15.1 Box<T>

`Box<T>` value'ni heapda saqlaydi:

```rust
let b = Box::new(5);
```

Stackda box pointer metadata turadi; actual `5` heapda. Scope tugaganda box va heap data cleaned up bo'ladi.

`Box<T>`ning eng muhim boshlang'ich use case'i recursive typelar:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

`Cons(i32, List)` infinite size muammosi beradi; `Cons(i32, Box<List>)` esa fixed pointer size orqali recursionni compiler uchun finite qiladi.

### 15.6 Reference Cycles va Weak<T>

`Rc<T>` + `RefCell<T>` kombinatsiyasi [[reference-cycle|reference cycle]] yaratishi mumkin — countlar 0 ga tushmasligi sababli [[memory-leak|memory leak]] yuzaga keladi. Rust bu holat ni kafolat qilmaydi (memory safe, lekin mantiqiy xato).

**`Weak<T>`** — yechim:

```rust
use std::rc::{Rc, Weak};

let rc = Rc::new(5);
let weak: Weak<i32> = Rc::downgrade(&rc); // weak_count++, strong_count o'zgarmaydi

// Weak dan foydalanish uchun upgrade
match weak.upgrade() {
    Some(val) => println!("{}", val),
    None      => println!("dropped"),
}
```

`weak_count` cleanup ga ta'sir qilmaydi — strong_count 0 bo'lsa yetarli.

**Tree qoidasi:** ota → bola `Rc<T>` (owns); bola → ota `Weak<T>` (non-owns):

```rust
struct Node {
    value:    i32,
    children: RefCell<Vec<Rc<Node>>>,  // strong
    parent:   RefCell<Weak<Node>>,     // weak
}
```

**Chapter 15 yakuniy xulosa:**

| Type | Ownership | Borrow checking | Mutable |
|---|---|---|---|
| `Box<T>` | bitta | compile time | ha |
| `Rc<T>` | ko'p | compile time | yo'q |
| `RefCell<T>` | bitta | runtime | ha |
| `Weak<T>` | yo'q | — | — |

### 15.4 Rc<T>

[[rc-t|Rc<T>]] bir qiymatga bir nechta o'quvchi egani ta'minlaydi — reference count orqali:

```rust
use std::rc::Rc;

let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
let b = Cons(3, Rc::clone(&a));   // count: 2
let c = Cons(4, Rc::clone(&a));   // count: 3
```

- `Rc::clone(&a)` — deep copy **emas**, faqat count++. Konventsiya bo'yicha `a.clone()` o'rniga yoziladi.
- `Rc::strong_count(&a)` — joriy kuchli referenslar soni.
- Faqat **immutable** access. Ko'p oqimli uchun `Arc<T>` (16-bob).
- `Drop` trait scope'dan chiqqanda countni avtomatik kamaytiradi.

### 15.5 RefCell<T> va Interior Mutability

[[interior-mutability|Interior mutability]] — immutable ko'rinadigan qiymatni ichkaridan o'zgartirish.

[[refcell-t|RefCell<T>]] borrow qoidalarini **runtime** da tekshiradi:
- `borrow()` → `Ref<T>` (immutable handle)
- `borrow_mut()` → `RefMut<T>` (mutable handle)
- Qoidalar buzilsa → `panic!` (compile error emas)

`Box<T>` / `Rc<T>` / `RefCell<T>` solishtirmasi:

| Type | Ownership | Checking | Mutable |
|------|-----------|----------|---------|
| `Box<T>` | bitta | compile | ha |
| `Rc<T>` | ko'p | compile | yo'q |
| `RefCell<T>` | bitta | runtime | ha |

**`Rc<RefCell<T>>`** — shared + mutable pattern:

```rust
let value = Rc::new(RefCell::new(5));
let a = Rc::clone(&value);
*value.borrow_mut() += 10;
println!("{}", a.borrow()); // 15
```

### 15.3 Drop

[[drop|Drop trait]] qiymat scope dan chiqib ketganda avtomatik ishlaydi. Prelude da bo'ladi; `drop(&mut self)` metodini implement qilish kerak:

```rust
impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}
```

Muhim qoidalar:
- O'zgaruvchilar **yaratilish tartibining teskarisida** drop qilinadi.
- `c.drop()` chaqirib bo'lmaydi — [[e0040-explicit-use-of-destructor|E0040]] xatosi; double free xavfi bor.
- Erta tozalash kerak bo'lsa, `drop(c)` funksiyasi (`std::mem::drop`, prelude da) ishlatiladi.

`Box<T>` `Drop` implement qilgani uchun scope tugaganda heap data ham avtomatik tozalanadi.

### 15.2 Deref

Oddiy reference:

```rust
let x = 5;
let y = &x;
assert_eq!(5, *y);
```

`Box<T>` ham `Deref` implement qilgani uchun `*box_value` ishlaydi. Custom `MyBox<T>` uchun esa `Deref` implement qilish kerak:

```rust
use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

`*y` compiler tomonidan `*(y.deref())` kabi ko'riladi. Deref coercion esa `&MyBox<String>`ni `&str` kutadigan functionga yuborishni ergonomic qiladi.

## Examples

- [[box-cons-list]]
- [[mybox-deref]]
- [[rc-shared-list]]
- [[tree-with-weak-parent]]

## Exercises

1. `enum List { Cons(i32, List), Nil }` yozib, [[e0072-recursive-type-has-infinite-size|E0072]] errorni izohlang.
2. `Box<List>` bilan fix qiling va stack/heap layoutni chizing.
3. `MyBox<T>`ni `Deref` implementationsiz yozing; keyin [[e0614-type-cannot-be-dereferenced|E0614]]ni `Deref` bilan tuzating.
4. `hello(&m)` va `hello(&(*m)[..])` variantlarini taqqoslang.
5. `&mut T -> &U` nima uchun mumkin, `&T -> &mut U` nima uchun mumkin emasligini borrowing rules bilan tushuntiring.
6. `CustomSmartPointer` yozib, bir necha instance yarating; drop tartibini terminal chiqishidan kuzating.
7. `c.drop()` chaqirib [[e0040-explicit-use-of-destructor|E0040]]ni ko'ring; `drop(c)` bilan to'g'rilang.
8. `Box<T>` bilan shared cons list yozishga urinib, E0382 xatosini ko'ring; `Rc<T>` bilan tuzating.
9. `Rc::strong_count`ni turli nuqtalarda chop etib, count qanday o'zgarishini kuzating.
10. `MockMessenger`ni `RefCell<Vec<String>>` bilan yozib, `&self` da `push` qiling.
11. `RefCell<T>`da ikkita `borrow_mut`ni bir scope da chaqirib, runtime panicni kuzating.
12. `Rc<RefCell<i32>>` bilan uch xil clone orqali bitta qiymatni o'zgartiring va hammasi ko'rishini tasdiqlang.
13. Listing 15-26 cycle yarating; `println!` bilan stack overflow kuzating.
14. Tree nodeiga `Weak<Node>` parent qo'shib, `branch` scope dan chiqqanda `leaf.parent.upgrade()` `None` qaytarishini tasdiqlang.
15. `Rc::strong_count` va `Rc::weak_count` ni Listing 15-29 jadvalini qo'lda takrorlang.

## Review Questions

1. Smart pointer oddiy referencedan nimasi bilan farq qiladi?
2. `Box<T>` qaysi uch holatda foydali?
3. Recursive type nima uchun infinite size muammosini chiqaradi?
4. `Deref::deref` nima qaytaradi va nega value qaytarmaydi?
5. Deref coercion runtime cost keltiradimi?
6. `Deref` va `DerefMut` qaysi coercion holatlarida ishlatiladi?
7. `Box<T>` nega smart pointer hisoblanadi?
8. Nima uchun `Drop::drop` metodini qo'lda chaqirib bo'lmaydi?
9. `std::mem::drop` va `Drop::drop` metodi qanday farq qiladi?
10. O'zgaruvchilar qaysi tartibda drop qilinadi?
11. `Rc::clone` va oddiy `clone` nimasi bilan farq qiladi?
12. `Rc<T>` mutable access bermasligi nima uchun?
13. `RefCell<T>` borrow qoidalarini buzganda nima bo'ladi?
14. `Rc<RefCell<T>>` qanday imkoniyat beradi?
15. `RefCell<T>` va `Mutex<T>` qanday bog'liq?
16. Nima uchun `Weak<T>` reference count ni 0 ga tushirish shart emas?
17. `Rc::downgrade` va `Weak::upgrade` qanday ishlaydi?
18. `strong_count` va `weak_count` orasidagi farq nima?
19. Ota-bola munosabatida nima uchun bola `Weak<T>` ishlatishi kerak?

## Related Pages

- [[wiki/sources/15-smart-pointers|Source: 15.0]]
- [[15-1-using-box-t-to-point-to-data-on-the-heap|Source: 15.1]]
- [[15-2-treating-smart-pointers-like-regular-references|Source: 15.2]]
- [[15-3-running-code-on-cleanup-with-the-drop-trait|Source: 15.3]]
- [[15-4-rc-the-reference-counted-smart-pointer|Source: 15.4]]
- [[15-5-refcell-t-and-the-interior-mutability-pattern|Source: 15.5]]
- [[15-6-reference-cycles-can-leak-memory|Source: 15.6]]
- [[smart-pointers]]
- [[box-t]]
- [[recursive-types]]
- [[deref-trait]]
- [[deref-mut-trait]]
- [[deref-coercions]]
- [[drop]]
- [[rc-t|Rc<T>]]
- [[refcell-t|RefCell<T>]]
- [[interior-mutability]]
- [[weak-t|Weak<T>]]
- [[reference-cycle]]
- [[memory-leak]]
- [[e0040-explicit-use-of-destructor|E0040]]

