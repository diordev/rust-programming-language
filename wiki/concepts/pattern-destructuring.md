---
title: "Pattern Destructuring"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, patterns, destructuring]
source_count: 3
---

# Pattern Destructuring

## Short Definition

Murakkab type (tuple, struct, enum) ni pattern orqali ichki qismlarga ajratib, har bir qismni alohida nomga bog'lash.

## Why It Matters

Destructuring kodni ixcham va expressivroq qiladi. Masalan `let (x, y, z) = point;` uch satr `let x = point.0; let y = point.1; ...`ni bir satrga siqadi. Funksiya parametrlarida ham qo'llash mumkin.

## Mental Model

"Qutini ochish" — bitta murakkab qiymatni bir nechta oddiy qiymatlarga bo'lish. Pattern qutining shaklini ta'riflaydi, nomlar esa ichidagi narsalarni ushlaydi.

Tuple positional, struct field-name asosida, array esa fixed-length shape asosida destructure qilinadi. Function parametrlari ham pattern position bo'lgani uchun destructuring signature'da ham ishlaydi.

## Syntax and Examples

**let bilan tuple destructuring:**
```rust
let (x, y, z) = (1, 2, 3);
// x=1, y=2, z=3
```

Element soni mos kelmasa compiler `E0308` beradi:
```rust
let (x, y) = (1, 2, 3);  // Xato: 3 element, 2 o'zgaruvchi
```

**for loop bilan tuple destructuring:**
```rust
let v = vec!['a', 'b', 'c'];
for (index, value) in v.iter().enumerate() {
    println!("{value} is at index {index}");
}
```

**Funksiya parametrida destructuring:**
```rust
fn print_coordinates(&(x, y): &(i32, i32)) {
    println!("({x}, {y})");
}

let point = (3, 5);
print_coordinates(&point);  // (3, 5)
```

**match bilan enum destructuring:**
```rust
match coin {
    Coin::Quarter(state) => println!("State: {state:?}"),
    _ => println!("other coin"),
}
```

**if let bilan Option destructuring:**
```rust
if let Some(value) = maybe {
    println!("{value}");
}
```

**Struct destructuring — rename va shorthand:**
```rust
// Field nomini o'zgartirish
let Point { x: a, y: b } = p;  // a = p.x, b = p.y

// Shorthand (nom bir xil)
let Point { x, y } = p;        // x = p.x, y = p.y

// Literal bilan aralash — field test + bind
match p {
    Point { x, y: 0 } => println!("x-axis at {x}"),
    Point { x: 0, y } => println!("y-axis at {y}"),
    Point { x, y } => println!("({x}, {y})"),
}
```

**Enum destructuring (barcha variant shakllari):**
```rust
match msg {
    Message::Quit => { ... }             // data yo'q
    Message::Move { x, y } => { ... }    // struct-like
    Message::Write(text) => { ... }      // tuple-like
    Message::ChangeColor(r, g, b) => { ... }
}
```

**Nested enum/struct:**
```rust
match msg {
    Message::ChangeColor(Color::Rgb(r, g, b)) => { ... }
    Message::ChangeColor(Color::Hsv(h, s, v)) => { ... }
    _ => ()
}
```

**Aralash (struct + tuple):**
```rust
let ((feet, inches), Point { x, y }) = ((3, 10), Point { x: 3, y: -10 });
```

**Array destructuring va tail binding:**
```rust
let [first, _, third, rest @ ..] = [1, 2, 3, 4, 5];
```

## Common Mistakes

- Tuple elementlari soni noto'g'ri → `E0308`.
- Destructuring paytida type mos kelmasa compiler xatosi.
- Struct fieldlari nomini yodlashni unutish (struct destructuring uchun).
- `match` ichida pattern scoping: field literal test qilish match'ni early-exit qilmaydi, lekin arm ketma-ket tekshiriladi (`Point { x: 0, y: 0 }` faqat birinchi mos armga tushadi).
- Oddiy destructuring bilan slice/vectorni ham xuddi arraydek ishlatish mumkin deb o'ylash.

## Related Concepts

- [[pattern-matching|pattern matching]]
- [[tuple]]
- [[array-destructuring|array destructuring]]
- [[structs]]
- [[enums]]
- [[irrefutable-pattern|irrefutable pattern]]
- [[match]]
- [[if-let|if let]]
- [[slice-patterns|slice patterns]]

## Sources

- [[wiki/sources/19-1-all-the-places-patterns-can-be-used]]
- [[wiki/sources/19-3-pattern-syntax]]
- [[wiki/sources/rust-for-backend-developers-destructuring]]
