---
title: "Rustda o'zgaruvchilar haqida"
type: question
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, question, variables]
source_count: 3
---

# Rustda o'zgaruvchilar haqida

## Answer

Rustda `variable`ni ko'pincha `binding` deb tushunish foydali: nom ma'lum bir value'ga bog'lanadi. Bu bog'lanish odatda `let` bilan yaratiladi.

```rust
let x = 5;
```

Eng muhim qoida: Rust variables default [[immutability|immutable]]. Demak, oddiy `let` bilan yaratilgan bindingga keyin boshqa qiymatni qayta assignment qilib bo'lmaydi.

### 1. `let`, `mut`, `const` o'rtasidagi farq

Rustda local variable uchun odatda uchta asosiy ko'rinish uchraydi:

```rust
let x = 5;                          // immutable binding
let mut count = 0;                  // mutable binding
const MAX_POINTS: u32 = 100_000;    // constant
```

- `let` yangi local binding yaratadi.
- `let mut` binding qiymatini keyin o'zgartirishga ruxsat beradi.
- `const` compile-time ma'lum bo'ladigan constant qiymat uchun ishlatiladi.

Ko'p kundalik Rust code'da local variable uchun `let` va `let mut` ishlatiladi; `const` esa ko'proq umumiy qoidalar, limitlar, yoki magic number o'rniga nomlangan qiymat berish uchun kerak bo'ladi.

### 2. Default immutability

Rust nima uchun variablesni default immutable qiladi? Sababi code reasoningni soddalashtirish. Agar binding o'zgarmasa, uni keyin qayerdadir yangilanib ketgan bo'lishidan xavotir kamroq bo'ladi. Bu ayniqsa keyingi [[ownership]], [[borrowing]], va concurrency mavzularida muhim.

Agar immutable bindingni o'zgartirmoqchi bo'lsangiz, compiler sizni to'xtatadi:

```rust
fn main() {
    let x = 5;
    // x = 6; // E0384
}
```

Bu holatda odatiy compiler xatosi [[e0384-cannot-assign-twice|E0384 cannot assign twice]] bo'ladi.

### 3. `mut` qachon ishlatiladi

Agar bindingning o'zi keyin boshqa qiymat olishi yoki ichidagi data mutatsiya qilinishi kerak bo'lsa, `mut` ishlatiladi:

```rust
let mut x = 5;
x = 6;
```

`mut` faqat compilerga ruxsat emas, balki code o'qiyotgan odamga ham signal: "bu qiymat keyin o'zgaradi."

Masalan:

```rust
let mut name = String::from("Rust");
name.push_str(" language");
```

Bu yerda binding ham mutable, `String` ichidagi buffer ham mutatsiya qilinmoqda.

Praktik misol sifatida guessing game'da:

```rust
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

`read_line` mavjud `String`ga yozgani uchun `guess` mutable bo'lishi va unga `&mut` reference berilishi kerak. Aks holda [[e0596-cannot-borrow-as-mutable|E0596 cannot borrow as mutable]] chiqadi.

### 4. `mut` nimani bermaydi

`mut` bindingni o'zgartirishga ruxsat beradi, lekin bindingning type'ini o'zgartirmaydi.

Quyidagi yondashuv ishlamaydi:

```rust
let mut spaces = "   ";
// spaces = spaces.len(); // E0308
```

Sababi `spaces` avval `&str`, keyin esa `usize` bo'lib qolmoqda. Bu esa [[e0308-mismatched-types|E0308 mismatched types]]ga olib keladi.

### 5. [[shadowing]] qachon kerak

Bir xil nomni saqlagan holda yangi binding yaratmoqchi bo'lsangiz, `shadowing` ishlatiladi. Bu mutation emas, yangi `let` binding:

```rust
let spaces = "   ";
let spaces = spaces.len();
```

Bu yerda birinchi `spaces` type'i `&str`, ikkinchisi esa `usize`.

`shadowing` foydali bo'ladigan holatlar:

- transformationdan keyin yangi type kelganda;
- eski qiymatning keyingi versiyasini shu nom bilan davom ettirmoqchi bo'lganda;
- immutable style'ni saqlab, bosqichma-bosqich ma'lumotni qayta ishlaganda.

Masalan:

```rust
let guess = "42";
let guess: u32 = guess.trim().parse().expect("number");
```

Bu pattern guessing game'da ham uchraydi: avval `String`, keyin parsed `u32`.

### 6. `const` nima va qachon ishlatiladi

`const` bilan e'lon qilingan [[constants|constant]]:

- har doim immutable bo'ladi;
- type annotation talab qiladi;
- compile-time expression bo'lishi kerak.

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

`const`ni local scope ichida ham, module darajasida ham yozish mumkin. Odatda UPPER_SNAKE_CASE convention ishlatiladi.

Muhim farq: `const` oddiy local variable emas. U ko'proq "nomlangan compile-time qiymat" sifatida ishlaydi. Shuning uchun "oddiy value saqlayman, keyin ishlataman" holatlarida `let`, "umumiy doimiy qoida" holatlarida `const` tanlanadi.

### 7. Type inference va type annotation

Rust ko'p joyda type'ni o'zi topadi:

```rust
let count = 42;
let is_ready = true;
```

Bu [[type-inference|type inference]] hisoblanadi. Lekin ba'zi joylarda compilerga yordam kerak bo'ladi:

```rust
let guess: u32 = "42".parse().expect("number");
```

Bu yerda `parse()` qaysi numeric type qaytarishi kerakligini annotation ko'rsatmoqda. Demak, qoida:

- context aniq bo'lsa, inference yetadi;
- context noaniq bo'lsa, [[type-annotations|type annotation]] yoziladi.

### 8. Scope: variable qayergacha yashaydi

Har bir bindingning [[scope]]i bor. U e'lon qilingan joydan keyin valid bo'ladi va scope tugaganda yo'qoladi:

```rust
{
    let s = String::from("hello");
    println!("{s}");
}
// bu yerda s ishlamaydi
```

Bu ayniqsa [[ownership]] bilan muhim: owner scope'dan chiqqanda value dropped bo'lishi mumkin.

### 9. Destructuring ham variable binding hisoblanadi

Rustda `let` faqat bitta nomga qiymat berish emas; u pattern bilan bir nechta binding ham yaratishi mumkin:

```rust
let (x, y) = (10, 20);
```

Bu [[tuple]] destructuring misoli. Keraksiz qismini ignore qilish ham mumkin:

```rust
let (x, _) = (10, 20);
let _unused = 5;
```

- `_` qiymatni ataylab ignore qilish uchun.
- `_unused` esa bindingni saqlaydi, lekin "unused variable" warningini yumshatadi.

### 10. Variables va ownership o'rtasidagi bog'lanish

Rustda variable mavzusi faqat `let` va `mut` bilan tugamaydi. Binding qaysi type'ni ushlab turganiga qarab assignment semantics ham o'zgaradi.

`Copy` types:

```rust
let x = 5;
let y = x;
println!("{x}, {y}");
```

`i32` kabi [[copy-trait|Copy trait]] typelar assignmentda copy bo'ladi, shuning uchun `x` valid qoladi.

Move types:

```rust
let s1 = String::from("hello");
let s2 = s1;
// println!("{s1}"); // E0382
```

`String` esa [[move-semantics|move semantics]] bilan ishlaydi: ownership `s1`dan `s2`ga o'tadi. Demak, "variable"ni tushunish Rustda "binding + ownership behavior"ni tushunish degani hamdir.

### 11. Amaliy tanlov qoidasi

Kundalik Rust code'da quyidagi qisqa qaror daraxti foydali:

- Qiymat o'zgarmasa: `let`
- Qiymat keyin o'zgarsa: `let mut`
- Yangi type yoki yangi bosqichdagi qiymat kerak bo'lsa: `shadowing`
- Compile-time doimiy qiymat bo'lsa: `const`

### 12. Eng ko'p uchraydigan xatolar

1. `mut`ni unutish

```rust
let name = String::from("Rust");
// name.push_str("!"); // xato
```

2. `mut` bilan type ham o'zgaradi deb o'ylash

```rust
let mut spaces = "   ";
// spaces = spaces.len(); // E0308
```

3. Immutable bindingdan mutable borrow olishga urinish

```rust
let guess = String::new();
// io::stdin().read_line(&mut guess); // E0596
```

4. `String` assignmenti oddiy copy deb o'ylash

```rust
let s1 = String::from("hi");
let s2 = s1; // move
```

Qisqa xulosa: Rustda variable oddiy "quti" emas. U nom, scope, mutability, type, va ko'p hollarda ownership semantics bilan birga keladi. `let`, `mut`, `shadowing`, va `const`ni yaxshi tushunsangiz, Rustning keyingi katta mavzulari ancha yengil ochiladi.

## Evidence

- [[variables-and-mutability|variables and mutability]]: Rust variables default immutable; `mut` o'zgarish intentini explicit qiladi.
- [[immutability]]: immutable binding qayta assignmentdan himoyalaydi.
- [[shadowing]]: `let` bilan yangi binding yaratadi va type o'zgarishiga imkon beradi.
- [[constants]]: `const` immutable va explicit type annotation talab qiladi.
- [[type-inference|type inference]]: ko'p bindinglarda type contextdan kelib chiqib aniqlanadi.
- [[type-annotations|type annotations]]: noaniq joylarda explicit type yozish kerak bo'ladi.
- [[scope]]: binding qayergacha valid ekanini va scope tugaganda nima bo'lishini belgilaydi.
- [[ownership]]: variable semantics Rustda memory boshqaruvi bilan bevosita bog'langan.
- [[copy-trait|Copy trait]]: `i32`, `bool` kabi typelar assignmentda copy bo'ladi.
- [[move-semantics|move semantics]]: `String` kabi typelar assignmentda ownershipni move qiladi.
- [[tuple]]: destructuring orqali bir nechta binding yaratish mumkin.
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]: immutable variablega qayta assignment qilinganda chiqadigan compiler error.
- [[e0596-cannot-borrow-as-mutable|E0596 cannot borrow as mutable]]: immutable bindingdan mutable borrow olishga uringanda chiqadi.
- [[wiki/sources/2-programming-a-guessing-game]]: `let mut guess` va `shadowing`ning amaliy ishlatilishi.
- [[3-1-variables-and-mutability]]: variables, mutability, constants va shadowingning asosiy manbasi.
- [[3-2-data-types]]: tuple destructuring va type annotationlar uchun tayanch.

## Follow-up

- `const` va `static` o'rtasidagi farqni alohida ko'rish.
- `mut` va [[shadowing]] orasidagi amaliy tanlovni ko'proq real misollar bilan mashq qilish.
- Variable bindinglar [[match]], [[if-let|if let]], va [[let-else|let...else]] ichida qanday ishlashini ko'rish.
- Variablesning [[ownership]], [[borrowing]], va [[lifetimes]] bilan bog'lanishini keyingi bosqichda chuqurlashtirish.

## Related Pages

- [[variables-and-mutability]]
- [[immutability]]
- [[shadowing]]
- [[constants]]
- [[type-inference]]
- [[type-annotations]]
- [[scope]]
- [[ownership]]
- [[copy-trait|Copy trait]]
- [[move-semantics|move semantics]]
- [[tuple]]
- [[e0384-cannot-assign-twice]]
- [[e0308-mismatched-types]]
- [[e0596-cannot-borrow-as-mutable]]
