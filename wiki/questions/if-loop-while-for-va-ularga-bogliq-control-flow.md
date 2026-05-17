---
title: "if, loop, while, for va ularga bog'liq control flow"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, control-flow]
source_count: 3
---

# if, loop, while, for va ularga bog'liq control flow

## Answer

Rustdagi `control flow` program qaysi yo'l bilan yurishini va qaysi block nechta marta bajarilishini boshqaradi. Bu mavzudagi asosiy vositalar:

- `if`
- `match`
- `loop`
- `while`
- `for`
- `break`
- `continue`
- `if let`
- `let...else`

Qisqa xarita:

- `if` bitta `bool` condition uchun
- `match` ko'p holat va patternlar uchun
- `loop` to'xtash sharti qo'lda boshqariladigan takrorlash uchun
- `while` condition true bo'lib turguncha takrorlash uchun
- `for` collection yoki range bo'ylab yurish uchun
- `break` loopni tugatish uchun
- `continue` current iterationni tashlab keyingisiga o'tish uchun

### `if`

`if` faqat `bool` condition oladi. Rustda truthy/falsy avtomatik konversiya yo'q.

```rust
if number != 0 {
    println!("number is not zero");
} else {
    println!("number is zero");
}
```

`if` expression hamdir:

```rust
let number = if condition { 5 } else { 6 };
```

Bu yerda ikkala arm bir xil type qaytarishi kerak. Agar `else if` ko'payib ketsa, kod chalkashishi mumkin; bunday joyda `match` ko'pincha tozaroq.

### `match`

`match` value'ni patternlar bilan solishtiradi va birinchi mos arm'ni bajaradi. Rust barcha holatlar yopilganini tekshiradi.

```rust
match number {
    1 => println!("one"),
    2 => println!("two"),
    _ => println!("something else"),
}
```

`enum`, `Option`, `Result`, `Ordering` bilan ishlashda ayniqsa kuchli. Faqat bitta case qiziq bo'lsa, `if let` ko'proq mos bo'ladi.

### `loop`

`loop` cheksiz takrorlaydi. Undan chiqish uchun `break` kerak.

```rust
loop {
    if ready {
        break;
    }
}
```

`break` value ham qaytarishi mumkin:

```rust
let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};
```

`loop` "qachon tugashini hozircha bilmayman" degan holatlarda foydali.

### `while`

`while` har iteration boshida condition tekshiradi. `false` bo'lsa loop tugaydi.

```rust
let mut n = 3;

while n != 0 {
    println!("{n}");
    n -= 1;
}
```

`while` conditionga bog'liq repetition uchun yaxshi. Collection bo'ylab yurishda manual index bilan yozish esa bounds xatolariga olib kelishi mumkin.

### `for`

`for` collection yoki range bo'ylab iteration uchun eng idiomatic yo'l.

```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("{element}");
}
```

Range bilan ham ishlaydi:

```rust
for number in (1..4).rev() {
    println!("{number}");
}
```

Ownership ham muhim:

```rust
for item in vec {
    println!("{item}");
}
```

Bu yerda `vec` move bo'lishi mumkin. Faqat o'qish kerak bo'lsa, odatda `for item in &vec` yoki `for item in vec.iter()` ishlatiladi. O'zgartirish kerak bo'lsa, `for item in &mut vec`.

### `break`, `continue`, loop labels

`break` loopni tugatadi. `continue` current iterationni tashlab keyingisiga o'tadi.

```rust
loop {
    let guess: u32 = match input.parse() {
        Ok(num) => num,
        Err(_) => continue,
    };

    if guess == secret {
        break;
    }
}
```

Nested looplarda label ishlatiladi:

```rust
'outer: loop {
    loop {
        break 'outer;
    }
}
```

### `if let` va `let...else`

Bu ikkisi `match`ning qisqartirilgan, pattern-markazli variantlari.

`if let`:

```rust
if let Some(max) = config_max {
    println!("max = {max}");
}
```

`let...else`:

```rust
let Some(name) = name else {
    return;
};
```

Farqi:

- `if let` faqat bitta pattern qiziq bo'lganda qulay
- `let...else` pattern bo'lmasa early return kerak bo'lganda aniqroq

### Qaysini qachon tanlash kerak

- `if` - bitta boolean qaror
- `else if` - bir nechta oddiy shart
- `match` - enum/pattern/multiple branches
- `loop` - qo'lda boshqariladigan repetition
- `while` - condition true bo'lgunga qadar
- `for` - collection/range iteration
- `if let` - bitta pattern qiziq bo'lsa
- `let...else` - pattern bo'lmasa early return kerak bo'lsa

### Eng ko'p uchraydigan xatolar

- `if` ga `bool` bo'lmagan qiymat berish
- `if` arms turli type qaytarishi
- `match`da barcha holatlarni yopmaslik
- collection uchun `while` + manual index ishlatib bounds xatosi qilish
- `loop`ga `break` qo'ymaslik va accidental infinite loop yaratish
- `for`da ownership move bo'lishini unutish
- `if let` ishlatib, aslida boshqa variantlar ham muhim bo'lgan holatni yashirish

## Evidence

- [[control-flow|control flow]]
- [[if-expressions|if expressions]]
- [[match]]
- [[loop]]
- [[while-loop|while loop]]
- [[for-loop|for loop]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[break]]
- [[continue]]
- [[loop-labels|loop labels]]
- [[3-5-control-flow]]
- [[6-2-the-match-control-flow-construct]]
- [[6-3-concise-control-flow-with-if-let-and-let-else]]

## Follow-up

- `for item in vec`, `for item in &vec`, va `for item in &mut vec` farqini alohida misollar bilan ko'rish.
- `match` va `if let`ni bir xil misolda solishtirish.
- Nested loop label'lar bilan kichik exercise yozish.

## Practice Examples

### 1. `if` va `match` farqi

Masala: sonning musbat, manfiy yoki nol ekanini chiqarish.

`if` bilan:

```rust
fn describe(n: i32) {
    if n > 0 {
        println!("positive");
    } else if n < 0 {
        println!("negative");
    } else {
        println!("zero");
    }
}
```

Bu yerda branchlar oddiy shartlar bo'lgani uchun `if` yetarli.

`match` bilan:

```rust
fn describe(n: i32) {
    match n.cmp(&0) {
        std::cmp::Ordering::Greater => println!("positive"),
        std::cmp::Ordering::Less => println!("negative"),
        std::cmp::Ordering::Equal => println!("zero"),
    }
}
```

Bu variant ko'proq structured ko'rinadi, ayniqsa holatlar ko'payganda.

### 2. `loop` + `break` + `continue`

Masala: foydalanuvchi to'g'ri parol kiritguncha qayta so'rash.

```rust
fn login() {
    let secret = "rust";

    loop {
        let input = "example";

        if input.is_empty() {
            continue;
        }

        if input == secret {
            println!("welcome");
            break;
        }

        println!("try again");
    }
}
```

Bu yerda:

- `continue` bo'sh inputni tashlaydi
- `break` to'g'ri javob bo'lganda loopni tugatadi

### 3. `while` va `for`ni bir xil masalada solishtirish

Masala: 1 dan 5 gacha sonlarni chiqarish.

`while` bilan:

```rust
fn print_with_while() {
    let mut n = 1;

    while n <= 5 {
        println!("{n}");
        n += 1;
    }
}
```

Bu ishlaydi, lekin siz `n`ni qo'lda boshqarasiz.

`for` bilan:

```rust
fn print_with_for() {
    for n in 1..=5 {
        println!("{n}");
    }
}
```

Bu qisqaroq va xavfsizroq. Counted loop uchun `for` odatda to'g'ri tanlov.

### 4. `for` va ownership

Masala: vector elementlarini o'qish.

```rust
fn read_items() {
    let items = vec!["a", "b", "c"];

    for item in &items {
        println!("{item}");
    }

    println!("{items:?}");
}
```

Bu yerda `&items` ishlatilgani uchun vector saqlanib qoladi. Agar `for item in items` yozilsa, `items` move bo'lishi mumkin.

## Related Pages

- [[control-flow|control flow]]
- [[if-expressions|if expressions]]
- [[match]]
- [[loop]]
- [[while-loop|while loop]]
- [[for-loop|for loop]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[break]]
- [[continue]]
- [[loop-labels|loop labels]]
