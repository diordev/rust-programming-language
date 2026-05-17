---
title: "15.5. RefCell<T> and the Interior Mutability Pattern"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, refcell, interior-mutability, borrowing]
source_count: 1
---

# 15.5. RefCell<T> and the Interior Mutability Pattern

## Source

`raw/books/the_rust_programming_language/15.5. RefCell<T> and the Interior Mutability Pattern.md`
URL: https://doc.rust-lang.org/stable/book/ch15-05-interior-mutability.html

## Detailed Summary

**Interior mutability** — immutable referenslar orqali ham ma'lumotni o'zgartira oladigan dizayn patterni. `RefCell<T>` bu patternni amalga oshiradi: borrow qoidalari compile time emas, **runtime** da tekshiriladi.

### Box/Rc vs RefCell — solishtirma

| Type | Ownership | Borrow tekshiruvi | Mutable |
|------|-----------|-------------------|---------|
| `Box<T>` | bitta | compile time | ha |
| `Rc<T>` | bir nechta | compile time | yo'q |
| `RefCell<T>` | bitta | **runtime** | ha |

Barchasi **single-threaded** (ko'p oqimli uchun `Mutex<T>`).

### Qachon RefCell<T> kerak

Kompilyator ba'zan to'g'ri kodni rad etadi — u "conservatively safe" ishlaydi. Agar siz borrow qoidalari bajarilishiga ishonchingiz komil bo'lsa, lekin kompilyator buni anglay olmasa, `RefCell<T>` ishlatiladi.

### borrow va borrow_mut

```rust
use std::cell::RefCell;

let data = RefCell::new(vec![1, 2, 3]);

// Immutable borrow → Ref<T>
let r1 = data.borrow();

// Mutable borrow → RefMut<T>
let r2 = data.borrow_mut();
```

`Ref<T>` va `RefMut<T>` — ikkalasi ham `Deref` implement qiladi, shuning uchun oddiy referens kabi ishlatiladi.

`RefCell<T>` ichki hisob yuritadi: nechta immutable (`Ref<T>`) va nechta mutable (`RefMut<T>`) borrow faol ekanligini kuzatadi. Compile time borrow qoidalari runtime da ham qo'llaniladi — buzilsa `panic!`.

### Mock object misoli

`Messenger` trait `&self` (immutable) bilan `send` chaqiradi. Oddiy struct bilan `Vec<String>` ichiga push qilib bo'lmaydi — E0596. `RefCell<Vec<String>>` esa bu muammoni hal qiladi:

```rust
struct MockMessenger {
    sent_messages: RefCell<Vec<String>>,
}

impl Messenger for MockMessenger {
    fn send(&self, message: &str) {
        // &self immutable bo'lsa ham, ichkarini mutate qilish mumkin
        self.sent_messages.borrow_mut().push(String::from(message));
    }
}

// Test ichida o'qish:
mock_messenger.sent_messages.borrow().len()
```

### Runtime panic misoli

Bir scope da ikkita `borrow_mut` chaqirilsa:

```rust
let one = self.sent_messages.borrow_mut();
let two = self.sent_messages.borrow_mut(); // PANIC: already borrowed
```

Xato: `already borrowed: BorrowMutError` — compile time emas, runtime da.

### Rc<RefCell<T>> kombinatsiyasi

`Rc<T>` immutable access beradi; `RefCell<T>` interior mutability beradi. Ikkalasini birlashtirish — ko'p egalik **va** o'zgartirish:

```rust
use std::cell::RefCell;
use std::rc::Rc;

let value = Rc::new(RefCell::new(5));
let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));
let b = Cons(Rc::new(RefCell::new(3)), Rc::clone(&a));
let c = Cons(Rc::new(RefCell::new(4)), Rc::clone(&a));

*value.borrow_mut() += 10;  // a, b, c hammasi 15 ko'radi
```

Bu "outwardly immutable, inwardly mutable" pattern.

## Key Concepts

- [[interior-mutability]] — immutable referens orqali mutate qilish patterni
- [[refcell-t|RefCell<T>]] — runtime borrow checking; `borrow()` / `borrow_mut()`
- `Ref<T>` — immutable borrow handle
- `RefMut<T>` — mutable borrow handle
- Runtime borrow xatosi → `panic!` (emas compile error)
- [[rc-t|Rc<T>]] + `RefCell<T>` = shared mutable ownership pattern

## Code Examples

Listing 15-20: `LimitTracker` va `Messenger` trait
Listing 15-21: `MockMessenger` — borrow checker bloklovchi variant
Listing 15-22: `RefCell<Vec<String>>` bilan to'g'ri MockMessenger
Listing 15-23: Ikkita `borrow_mut` — runtime panic
Listing 15-24: `Rc<RefCell<i32>>` — shared mutable list

## Exercises or Practice Ideas

1. `MockMessenger`ni `RefCell` bilan yozib, testni o'tkazing.
2. Bir scope da ikkita `borrow_mut` chaqirib, panicni kuzating.
3. `Rc<RefCell<i32>>` bilan cons list yozib, shared mutationni tekshiring.
4. `Box<T>`, `Rc<T>`, `RefCell<T>` uchun minimal misollar yozib, farqlarini taqqoslang.

## Questions Raised

- `RefCell<T>` multithreaded kontekstda nima beradi? (`Mutex<T>` analogiga o'tish — Chapter 16)
- Halting Problem bilan "conservative static analysis" qanday bog'liq?
- `Cell<T>` nima va `RefCell<T>`dan qanday farq qiladi?

## Links To Update

- [[refcell-t]] — yangi concept sahifasi yaratish
- [[interior-mutability]] — yangi concept sahifasi yaratish
- [[rc-t]] — Rc+RefCell kombinatsiyasi qo'shish
- [[wiki/chapters/15-smart-pointers]] — 15.4, 15.5 bo'limlarini qo'shish
- [[index]] — yangi sourcelar qo'shish
