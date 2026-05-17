---
title: "21.2. From Single-Threaded to Multithreaded Server"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, web-server, thread-pool, concurrency]
source_count: 1
---

# 21.2. From Single-Threaded to Multithreaded Server

## Source

- File: `raw/books/the_rust_programming_language/21.2. From Single-Threaded to Multithreaded Server.md`
- URL: <https://doc.rust-lang.org/stable/book/ch21-02-multithreaded.html>
- Book: *The Rust Programming Language*

## Detailed Summary

21.2 bo'limi 21.1 dagi single-threaded serverning real bottleneck'ini ko'rsatib, uni fixed-size [[thread-pool]] bilan concurrent serverga aylantiradi. Bu bo'limning kuchli tomoni shundaki, u birdan "to'g'ri final design"ni bermaydi; muammo, naive yechim, keyin resource-safe yechim tartibida yuradi.

### Slow request muammosini ko'rsatish

Bo'lim avval `GET /sleep HTTP/1.1` case'ini qo'shadi va `thread::sleep(Duration::from_secs(5))` bilan sekin request'ni sun'iy simulyatsiya qiladi. Natija: bir `*/sleep*` request kelganda, undan keyingi oddiy `*/` request'lar ham kutib qoladi. Bu single-threaded [[web-server]] modelining asosiy muammosi: bitta blocking request butun server throughput'ini pasaytiradi.

### Har request uchun yangi thread

Keyingi qadam sifatida book har incoming stream uchun `thread::spawn(move || handle_connection(stream))` ishlatadi. Bu serverni darhol concurrent qiladi: `*/sleep*` ishlayotgan paytda boshqa request'lar boshqa threadlarda bajarilishi mumkin.

Lekin bu final design emas. Har request uchun alohida thread spawn qilish cheksiz resurs sarfiga olib keladi. Agar tashqaridan juda ko'p request kelsa, server thread sonini cheksiz oshirib, DoS'ga yaqin resource exhaustion holatiga tushishi mumkin.

### Fixed-size thread pool g'oyasi

Book keyin to'g'ri direction'ni belgilaydi: oldindan spawn qilingan, kutib turgan ma'lum sonli worker threadlar havzasi. Request kelganda yangi thread yaratish o'rniga, ish shu workerlardan biriga topshiriladi. Qolgan request'lar esa queue'da kutadi.

Bu design uchta muhim foyda beradi:

- throughput oshadi, chunki birdaniga bir nechta request bajarilishi mumkin;
- thread soni cheklangan, shu sababli resource usage boshqariladi;
- request backlog workerlar sonidan oshsa, ular queue'da tartibli kutadi.

### Compiler-driven development

Bo'lim implementation'ni [[compiler-driven-development]] usulida olib boradi. Avval desired API yoziladi:

```rust
let pool = ThreadPool::new(4);

pool.execute(|| {
    handle_connection(stream);
});
```

Keyin `cargo check` xatolari bo'yicha `ThreadPool`, `new`, `execute`, va ichki design asta-sekin quriladi. Bu usul TDDga o'xshaydi, lekin testlar o'rniga compiler feedback'idan foydalanadi.

### `ThreadPool::execute` signaturasi

`execute` qabul qiladigan closure `thread::spawn`ga o'xshab quyidagi bound'larni oladi:

- `FnOnce()` - job bir marta bajariladi;
- `Send` - closure boshqa threadga ko'chiriladi;
- `'static` - worker thread uni qancha vaqt ushlab turishi noma'lum, shuning uchun qisqa borrow'lar bilan bog'liq bo'lmasligi kerak.

Bu keyinchalik `type Job = Box<dyn FnOnce() + Send + 'static>;` aliasiga olib keladi.

### `new(size)` validatsiyasi

`ThreadPool::new(size: usize)` ichida `assert!(size > 0)` qo'shiladi. Manfiy thread soni allaqachon `usize` bilan rad etilgan, lekin `0` ham ma'nosiz. Book bu joyda `Result` emas, panic tanlaydi va bu qarorni API-level tradeoff sifatida muhokama qiladi.

### Worker struct va JoinHandle

Dastlab `ThreadPool` `Vec<JoinHandle<()>>` saqlashi mumkinligi ko'rsatiladi, lekin keyin design `Worker` struct'iga ko'chadi:

- `id: usize`
- `thread: JoinHandle<()>`

Bu worker'larni debug/log uchun identifikatsiya qilishni osonlashtiradi va ichki implementationni tozalaydi.

### Channel orqali job queue

`execute` ichida kelgan closure'ni to'g'ridan-to'g'ri threadga uzatib bo'lmaydi, chunki worker threadlar oldindan yaratilgan va keyinroq ish kutishi kerak. Shu sababli book [[channels]]ni job queue sifatida ishlatadi:

- `ThreadPool` `mpsc::Sender<Job>` saqlaydi;
- workerlar `Receiver<Job>`dan job olib ishlaydi.

Bu `Job` type alias bilan yoziladi:

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;
```

`execute` closure'ni `Box::new(f)` qilib yuboradi, worker esa uni `job();` deb bajaradi.

### `Receiver`ni workerlar orasida bo'lishish

Bu yerda muhim ownership muammosi chiqadi. `mpsc::Receiver<Job>` `Copy` emas va uni `for` loop ichida har workerga ownership bilan berib bo'lmaydi; compiler `E0382 use of moved value: receiver` beradi. Bundan ham muhimrog'i, std `mpsc` single-consumer model: receiver'ni oddiy clone qilib bo'lmaydi.

Yechim:

```rust
let receiver = Arc::new(Mutex::new(receiver));
```

Keyin har worker `Arc::clone(&receiver)` oladi. [[arc-t|Arc<T>]] shared ownership beradi, [[mutex-t|Mutex<T>]] esa bir vaqtda faqat bitta worker channel'dan job olishini kafolatlaydi.

### Worker execution loop

Worker thread ichidagi final loop:

```rust
loop {
    let job = receiver.lock().unwrap().recv().unwrap();
    println!("Worker {id} got a job; executing.");
    job();
}
```

Bu yerda `recv()` blocking bo'ladi; ish bo'lmasa worker kutadi. Job kelganda, u bajariladi va loop davom etadi.

### `while let` varianti nega noto'g'ri

Bo'lim nihoyat juda nozik, lekin amaliy detailni ko'rsatadi. Quyidagi kod compile bo'ladi:

```rust
while let Ok(job) = receiver.lock().unwrap().recv() {
    job();
}
```

Lekin u lock'ni kerakdan uzoq ushlab turadi. Sabab: `while let` ichidagi temporary `MutexGuard` block tugaguncha yashaydi. Natijada `job()` bajarilayotgan paytda ham lock qo'yilmaydi va boshqa workerlar yangi job ola olmaydi. Shu sababli server amalda yana serial behavior'ga yaqinlashadi.

To'g'ri variantdagi `let job = ...;` esa `MutexGuard`ni statement tugashi bilan drop qiladi. Bu [[mutexguard-lifetime]] nuancesi bo'lib, RAII va temporary lifetime'larni noto'g'ri baholash concurrency'ni buzib qo'yishi mumkinligini ko'rsatadi.

## Key Concepts

- [[thread-pool]]
- [[worker-thread]]
- [[job-queue]]
- [[compiler-driven-development]]
- [[mutexguard-lifetime]]
- [[channels]]
- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[fn-traits|Fn traits]]
- [[send-trait|Send trait]]
- [[static-lifetime|static lifetime]]
- [[type-alias]]

## Code Examples

- [[slow-sleep-request|Listing 21-10: slow /sleep request]]
- [[per-request-thread-spawn|Listing 21-11: per-request thread::spawn]]
- [[threadpool-new-and-execute|Listing 21-12: ideal ThreadPool API]]
- [[arc-mutex-mpsc-receiver|Listing 21-18: Arc<Mutex<mpsc::Receiver<Job>>>]]
- [[job-boxed-fnonce-alias|Listings 21-19 and 21-20: Job alias and execute loop]]
- [[while-let-mutexguard-pitfall|Listing 21-21: while let MutexGuard pitfall]]

## Exercises or Practice Ideas

1. `GET /sleep HTTP/1.1` case'i bilan single-threaded va thread-per-request variantni taqqoslang.
2. `ThreadPool::new(0)` nega panic qilishini va bu joyda `Result` qaytarish qachon ma'qul bo'lishini yozing.
3. `FnOnce() + Send + 'static` bound'larining har birini alohida misol bilan tushuntiring.
4. `mpsc::Receiver<Job>`ni clone qilib bo'lmasligining ikkita sababini yozing: ownership va API design.
5. `while let` varianti nega compile bo'lsa ham yomon concurrency beradi, satrma-satr tahlil qiling.
6. `thread::Builder` bilan `spawn`ni `Result` qaytaradigan shaklda qanday ishlatishni o'rganing.

## Questions Raised

- Thread pool uchun eng yaxshi size qanday tanlanadi: CPU count, I/O xususiyati, yoki boshqa telemetry?
- Graceful shutdown bo'lsa worker loop va channel protocol qanday o'zgaradi?
- Futures bilan xuddi shu design qaysi joylarda o'zgarar, qaysi joylarda saqlanib qoladi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
- [[thread-pool]]
- [[worker-thread]]
- [[job-queue]]
