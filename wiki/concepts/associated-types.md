---
title: "Associated Types"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, traits, generics, types]
source_count: 2
---

# Associated Types

## Short Definition

Trait ichida `type Name;` orqali e'lon qilinadigan **type placeholder**. Trait method'lari signaturasida ishlatiladi; har implement qiluvchi tip aniq concrete type tanlaydi.

## Why It Matters

Generics bilan o'xshash, lekin muhim farq: associated type bilan **bitta tip uchun bitta implementatsiya** mumkin. Generic bilan `Iterator<u32> for Counter` va `Iterator<String> for Counter` — ikkalasi mumkin, lekin caller har chaqiriqda type annotation berishi kerak. Associated type bilan `Counter` faqat bitta `Iterator` bo'lishi mumkin, va Item turi avtomatik aniqlanadi.

## Mental Model

```
Generic param  →  trait_user(Counter)  +  type annotation
Associated     →  trait_user(Counter)  →  type avtomatik
```

Generic — caller tanlaydi. Associated — implementor tanlaydi (faqat bir marta).

Backend beginner source bu farqni generic trait bilan to'g'ridan-to'g'ri ko'rsatadi. `trait CanBeAccessed<T>` modelida usage tarafida qaysi `T` ishlayotgani muhim; `trait CanBeAccessed { type ElementType; ... }` modelida esa implementor `Holder<T>` uchun `type ElementType = T;` deb bir marta belgilaydi. Yana muhim signal: generic trait bir xil concrete type uchun bir nechta impl olishi mumkin, associated type esa bitta trait impl ichida bitta bog'langan type beradi.

## Syntax and Examples

### `Iterator` traiti — klassik misol

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

`Item` — placeholder. Implement qilganda concrete type beriladi:

```rust
struct Counter { count: u32 }

impl Iterator for Counter {
    type Item = u32;   // <-- placeholder o'rniga aniq tip

    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}
```

`Self::Item` — bu implementatsiyada `u32`. Foydalanuvchi `counter.next()` chaqirsa — qaytishi `Option<u32>`.

### Generic bilan taqqoslash (gipotetik)

```rust
// Agar Iterator generic bilan yozilgan bo'lsa:
pub trait Iterator<T> {
    fn next(&mut self) -> Option<T>;
}

// Bu mumkin bo'lardi:
impl Iterator<u32>    for Counter { /* ... */ }
impl Iterator<String> for Counter { /* ... */ }
// Va caller:
let item: u32 = <Counter as Iterator<u32>>::next(&mut counter);
```

Associated type bilan ikkita implementatsiya mumkin emas → caller boshini qotirmaydi.

Backend-style example:

```rust
trait CanBeAccessed {
    type ElementType;
    fn get(&self) -> &Self::ElementType;
}

struct Holder<T> {
    v: T,
}

impl<T> CanBeAccessed for Holder<T> {
    type ElementType = T;

    fn get(&self) -> &Self::ElementType {
        &self.v
    }
}
```

## Qachon Associated Type, Qachon Generic

| Mezon | Associated Type | Generic Param |
|-------|------------------|----------------|
| Bir tip uchun bitta implementatsiya | ✓ | ✗ |
| Bir tip uchun ko'p implementatsiya | ✗ | ✓ |
| Caller tip annotatsiyasini bermasligi | ✓ | ✗ (ko'pincha) |
| API contract'i tip nomini fix qiladi | ✓ | ✗ |
| Maksimal moslashuvchanlik | ✗ | ✓ |

Misol:
- `Iterator::Item` — har iterator bittagina elementni qaytaradi → associated.
- `Add<Rhs>` — `Point + Point` va `Millimeters + Meters` ikkalasi bo'lishi mumkin → generic (default value bilan).

## Common Mistakes

- **`Self::Item` o'rniga `Item` yozish.** Trait body ichida `Item` o'zi referensli emas, `Self::Item` kerak.
- **Associated type bo'lganda generic ishlatishga urinish.** API mantiqan bitta implementatsiya bo'lsa — associated to'g'ri.
- **Implementatsiya'da `type Name = ...` ni unutish.** Compiler shikoyat qiladi: "missing type for associated type."
- Generic trait bilan associated type bir xil multiplicity beradi deb o'ylash.

## Related Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[trait-bounds|trait bounds]]
- [[iterators]] — `Iterator::Item` eng keng tarqalgan misol
- [[default-generic-parameters|default generic parameters]] — generic bilan birlashadi
- [[fully-qualified-syntax|fully qualified syntax]] — disambiguation

## Sources

- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[13-2-processing-a-series-of-items-with-iterators|13.2 Iterators]]
- [[wiki/sources/rust-for-backend-developers-generics]]
