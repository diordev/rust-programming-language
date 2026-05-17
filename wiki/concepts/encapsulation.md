---
title: "Encapsulation"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, design]
source_count: 1
---

# Encapsulation

## Short Definition

Encapsulation — ob'ektning implementatsiya tafsilotlarini yashirish; faqat public API orqali muloqot. Rust `pub`/private mexanizmi orqali to'liq kapsullashni ta'minlaydi.

## Why It Matters

Encapsulation ikki asosiy foyda beradi:
1. **Invariant himoya** — implementatsiya qo'riqchi metodlar orqali o'tadi, notog'ri holat imkonsiz
2. **Implementatsiya erkinligi** — ichki tuzilma o'zgarganda klient kodi o'zgarmaydi

## Mental Model

ATM mashina: tugmalarni bosishingiz mumkin (public API), lekin ichidagi kassani to'g'ridan-to'g'ri ushlamasligingiz kerak (private implementation). Bank kassiri ham doim xuddi shunday ishlaydi — hisob saldosini faqat ruxsat etilgan operatsiyalar orqali o'zgartirish mumkin.

## Syntax and Examples

### AveragedCollection — kanonik namuna

```rust
pub struct AveragedCollection {
    list: Vec<i32>,   // private: faqat metod orqali o'zgartiriladi
    average: f64,     // private: doim list bilan sinxron saqlanadi
}

impl AveragedCollection {
    // Public API — yagona kirish nuqtalari:
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();  // invariant: average doim yangilanadi
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(v) => { self.update_average(); Some(v) }
            None => None,
        }
    }

    pub fn average(&self) -> f64 { self.average }

    // Private helper — klient kodi chaqira olmaydi:
    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

**Invariant**: `list` o'zgarganda `average` doim `update_average` orqali yangilanadi. Klient kod `list.push()` to'g'ridan-to'g'ri chaqira olmaydi — invariant buzilolmaydi.

### Implementatsiya o'zgarishi — klient kod o'zgarmaydi

```rust
// Ichki tuzilma Vec → HashSet ga o'zgarsa:
pub struct AveragedCollection {
    list: HashSet<i32>,  // o'zgardi
    average: f64,
}
// Lekin public API (add, remove, average) o'zgarmaydi
// → klient kodi qayta yozilmaydi
```

### Module darajasidagi encapsulation

```rust
mod bank {
    pub struct Account { balance: f64 }  // balance private
    impl Account {
        pub fn deposit(&mut self, amount: f64) { self.balance += amount; }
        pub fn balance(&self) -> f64 { self.balance }
    }
}

// Klient:
let mut acc = bank::Account { balance: 0.0 };  // XATO: balance private
let mut acc = bank::Account::new(0.0);          // ✓
```

## Common Mistakes

- **Barcha fieldlarni pub qilish**: invariantlar buziladi, implementatsiya o'zgarsa klient kodi o'zgaradi.
- **Faqat getter/setter qo'shish va fieldni private qilish**: bu encapsulation emas — behaviourni ham modellash kerak.

## Related Concepts

- [[privacy]] — Rust'dagi pub/private mexanizmi
- [[pub-keyword|pub keyword]] — kirish nazorati
- [[public-api|public API]] — foydalanuvchi bilan shartnoma
- [[polymorphism]] — encapsulation bilan birga OOP asosi
- [[invariants]] — encapsulation himoya qiladigan narsalar

## Sources

- [[wiki/sources/18-1-characteristics-of-object-oriented-languages]]
