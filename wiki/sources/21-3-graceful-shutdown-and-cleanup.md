---
title: "21.3. Graceful Shutdown and Cleanup"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, thread-pool, cleanup, drop, concurrency]
source_count: 1
---

# 21.3. Graceful Shutdown and Cleanup

## Source

- File: `raw/books/the_rust_programming_language/21.3. Graceful Shutdown and Cleanup.md`
- URL: <https://doc.rust-lang.org/stable/book/ch21-03-graceful-shutdown-and-cleanup.html>
- Book: *The Rust Programming Language*

## Detailed Summary

21.3 bo'limi 21.2 dagi ishlaydigan [[thread-pool]]ni "to'g'ri tugatish" muammosiga olib keladi. Thread pool request'larni concurrent bajaradi, lekin dastur scope'dan chiqayotganda worker threadlarni shunchaki tashlab yuborish noto'g'ri: ular hali request o'rtasida bo'lishi mumkin. Shu sabab bo'lim [[graceful-shutdown]] va cleanup lifecycle'ini ownership, [[drop]], channel closure, va `join` bilan quradi.

### Nima uchun cleanup kerak?

21.2 kodi request'larni asynchronous bajaradi, lekin `ctrl-C` yoki main thread tugashi bilan qolgan threadlar keskin to'xtab qoladi. Bu active request'larni yarim yo'lda tashlab qo'yishi mumkin. 21.3 shuni tuzatadi: pool drop bo'lganda workerlar oldin o'z ishini tugatishi, keyin orderly shutdown qilishi kerak.

### `Drop` orqali lifecycle hook

Book avval `impl Drop for ThreadPool` yozib, har worker thread ustida `join` chaqirishga urinadi. G'oya sodda: pool scope'dan chiqsa, Rust avtomatik `drop` chaqiradi, va o'sha yerda threadlar tugashini kutish mumkin.

### `join` ownership oladi va `E0507`

Birinchi urinish:

```rust
for worker in &mut self.workers {
    worker.thread.join().unwrap();
}
```

Compiler `E0507 cannot move out of worker.thread which is behind a mutable reference` beradi. Sabab: `JoinHandle::join(self)` handle ownership'ini oladi. `&mut worker` orqali field'dan owned value'ni shunchaki yulib olib bo'lmaydi.

Bu joyda book ownershipni chiqarishning ikki patternini ko'rsatadi:

- narrative fix sifatida `self.workers.drain(..)` bilan workerlarni vector'dan ownership bilan olish;
- final consolidated code'da esa `Worker { thread: Option<JoinHandle<()>> }` va `worker.thread.take()` patterni.

Demak 21.3 nafaqat graceful shutdown haqida, balki cleanup paytida ownership'ni struct ichidan qanday xavfsiz chiqarish haqida ham.

### `Vec::drain(..)` roli

`Vec::drain(..)` butun vector ichidagi elementlarni ownership bilan iterator sifatida chiqaradi. Shu sabab `Drop` ichida:

```rust
for worker in self.workers.drain(..) {
    worker.thread.join().unwrap();
}
```

ishlaydi. Bu `worker`larni borrowed emas, owned holatda beradi.

### Nega faqat `join` yetarli emas?

Agar workerlar ichidagi loop hali ham:

```rust
loop {
    let job = receiver.lock().unwrap().recv().unwrap();
    job();
}
```

ko'rinishida qolsa, ular cheksiz `recv()` kutaveradi. Demak main thread `join()` qilganda abadiy bloklanib qoladi. Shutdown uchun workerlarga "endi yangi job bo'lmaydi" degan signal ham kerak.

### `sender`ni yopish orqali shutdown signali

Book keyin `ThreadPool.sender`ni:

```rust
sender: Option<mpsc::Sender<Job>>
```

ga o'zgartiradi va `Drop` ichida:

```rust
drop(self.sender.take());
```

chaqiradi. Bu yerda [[option|Option]] ichidagi sender ownership bilan olinadi va drop qilinadi. Channelning sending uchi yopilgach, worker tarafdagi `recv()` endi yangi job kutib turmaydi; u `Err` qaytaradi.

Bu pattern nega `Option` talab qilishini book aniq ko'rsatadi: `drop` ichida `self.sender`ni owned qiymat sifatida ko'chirish kerak, lekin `&mut self` bilan to'g'ridan-to'g'ri move qilib bo'lmaydi. `Option::take()` esa joyida `None` qoldirib ownership beradi.

### Worker loop'ni `recv()` xatosi bilan tugatish

Worker endi `recv().unwrap()` qila olmaydi. O'rniga:

```rust
let message = receiver.lock().unwrap().recv();

match message {
    Ok(job) => job(),
    Err(_) => break,
}
```

ko'rinishiga o'tadi. `Err(_)` bu kanal yopilganini bildiradi, ya'ni pool endi yangi job yubormaydi. Worker loopdan chiqadi, thread tugaydi, va `join()` uni toza kutib oladi.

### `listener.incoming().take(2)` demo

Bo'lim graceful shutdown'ni ko'rsatish uchun web serverni sun'iy ravishda ikki requestdan keyin to'xtatadi:

```rust
for stream in listener.incoming().take(2) {
    ...
}
```

`Iterator::take(2)` serverni real product behavior uchun emas, shutdown code ishlashini namoyish qilish uchun ishlatiladi. Pool scope'dan chiqadi, `Drop` chaqiriladi, sender yopiladi, workerlar `recv()`dan `Err` olib loopdan chiqadi, va main thread ularni `join()` qiladi.

### Yakuniy takeaway

21.2 thread pool'ni "ishlaydigan" holatga olib kelgan bo'lsa, 21.3 uni "to'g'ri tugaydigan" holatga olib keladi. Bu bo'lim ownership, lifetime, channel semantics, va cleanup hooks concurrency kodda qanchalik amaliy ekanini ko'rsatadi.

## Key Concepts

- [[graceful-shutdown]]
- [[thread-pool]]
- [[drop]]
- [[join-handle]]
- [[channels]]
- [[option]]
- [[vector|Vec<T>]]
- [[e0507-cannot-move-out-of-borrowed-content|E0507]]

## Code Examples

- [[threadpool-drop-and-join|Drop for ThreadPool and join]]
- [[sender-option-take-shutdown|sender: Option<Sender<Job>> + drop(self.sender.take())]]
- [[worker-recv-disconnect-shutdown|worker recv() Err shutdown]]
- [[incoming-take-two-shutdown-demo|listener.incoming().take(2) graceful shutdown demo]]

## Exercises or Practice Ideas

1. `join()` nega `&mut self` bilan emas, owned `JoinHandle` bilan ishlashini izohlang.
2. `Option::take()` va `Vec::drain(..)` ownership chiqarish uchun qaysi vaziyatlarda qulayroq ekanini taqqoslang.
3. `sender`ni drop qilmasdan `join()` qilsangiz nima bo'lishini satrma-satr tushuntiring.
4. `recv()` xatosini `unwrap()` bilan qoldirish graceful shutdown'ni nega buzishini yozing.
5. `take(2)` demosini `take(1)` va `take(5)` bilan fikran sinab ko'ring.

## Questions Raised

- Real server uchun graceful shutdown'da in-flight request timeout yoki cancellation policy kerak bo'ladimi?
- `Drop` ichida panic qilish production thread pool uchun qanday xavf keltiradi?
- Shutdown signalini channel closure o'rniga explicit enum message bilan yuborishning afzalliklari bormi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/21-3-graceful-shutdown-and-cleanup|Chapter 21.3]]
- [[graceful-shutdown]]
- [[e0507-cannot-move-out-of-borrowed-content|E0507]]
