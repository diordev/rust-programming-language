---
title: "RefCell<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, refcell, interior-mutability, borrowing]
source_count: 1
---

# RefCell<T>

## Short Definition

`RefCell<T>` — borrow qoidalarini compile time emas, **runtime** da tekshiradigan smart pointer. Interior mutability patternini amalga oshiradi: tashqaridan immutable ko'rinadigan qiymatni ichkaridan o'zgartirish imkonini beradi.

## Why It Matters

Kompilyator ba'zan to'g'ri kodni rad etadi — u "conservatively safe" ishlaydi (Halting Problem). Siz borrow qoidalari bajarilishiga ishonchingiz komil bo'lsa-yu, kompilyator buni anglay olmasa, `RefCell<T>` ishlatiladi.

Amaliy holat: `&self` (immutable) oladigan trait metodida ichki state ni o'zgartirish kerak bo'lganda — masalan, mock objects.

**Cheklov:** faqat **single-threaded**. Ko'p oqimli uchun `Mutex<T>`.

## Mental Model

`Box<T>` — kapitalchi qorovul (compile time, qattiq).
`RefCell<T>` — runtime qorovul (runtime, yumshoq lekin panics bilan jazolaydigan).

Qoidalar bir xil: bir vaqtda ya bitta mutable, yoki ko'plab immutable borrow. Lekin buzilsa compile xatosi emas — runtime `panic!`.

## Syntax and Examples

```rust
use std::cell::RefCell;

let data = RefCell::new(vec![1, 2, 3]);

// Immutable borrow → Ref<T>
{
    let r = data.borrow();
    println!("{:?}", *r); // [1, 2, 3]
} // r dropped, borrow released

// Mutable borrow → RefMut<T>
{
    let mut r = data.borrow_mut();
    r.push(4);
} // r dropped
```

Mock object patterni:

```rust
use std::cell::RefCell;

struct MockMessenger {
    sent_messages: RefCell<Vec<String>>,
}

impl Messenger for MockMessenger {
    fn send(&self, msg: &str) {
        // &self immutable bo'lsa ham
        self.sent_messages.borrow_mut().push(msg.to_string());
    }
}
```

`Rc<RefCell<T>>` — shared mutable ownership:

```rust
use std::rc::Rc;
use std::cell::RefCell;

let shared = Rc::new(RefCell::new(5));
let a = Rc::clone(&shared);
let b = Rc::clone(&shared);

*shared.borrow_mut() += 10;
println!("{}", a.borrow()); // 15
println!("{}", b.borrow()); // 15
```

## Common Mistakes

- Runtime panic kutmasdan ikkita `borrow_mut` chaqirish: `already borrowed: BorrowMutError`.
- `RefCell<T>` ni multithreaded kodda ishlatish — compile xatosi; o'rniga `Mutex<T>`.
- `borrow()` va `borrow_mut()` ni bir scope da aralashtirib yuborish.

## Related Concepts

- [[interior-mutability]] — patternning o'zi
- [[rc-t|Rc<T>]] — `Rc<RefCell<T>>` kombinatsiyasi
- [[borrowing]] — qoidalar runtime da ham bir xil
- [[smart-pointers]]
- [[box-t|Box<T>]] — compile time analogu
- [[unsafe-rust]] — `RefCell<T>` ichkarida unsafe code ishlatadi

## Sources

- [[15-5-refcell-t-and-the-interior-mutability-pattern]]
