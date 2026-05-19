---
title: "Async Task"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-19
tags: [rust, async, concurrency]
source_count: 2
---

# Async Task

## Short Definition

Async task — runtime tomonidan boshqariladigan yengil concurrent ish birligi. `trpl::spawn_task` orqali yaratiladi; OS thread'i emas, lekin `thread::spawn` ga o'xshash API.

## Why It Matters

Thread yaratish OS resurslari sarflaydi — har bir thread o'zining stack xotirasini oladi (odatda 1–8 MB). Async task esa ancha yengil: bir necha kilobyte. Bu bir vaqtda minglab async task boshqarish imkonini beradi, holbuki minglab thread yaratish amalda mumkin emas.

## Mental Model

Thread'ni alohida ishchi sifatida tasavvur qiling — har birining ish stoli (stack) bor. Task esa bitta ishchi o'z ish stoliga yozib qo'yadigan qog'oz parcha — u stolni egallamas, istalgan vaqt qo'yib ketishi mumkin.

Past darajada task odatda future va uni qayta queue'ga qo'yadigan [[waker|Waker]] bilan bog'lanadi. Custom executor misollarida task `Pin<Box<dyn Future<Output = ()>>>` va queue senderini o'zida saqlashi mumkin.

| | Thread | Async Task |
|---|---|---|
| Yaratish | `thread::spawn(closure)` | `spawn_task(async { })` |
| Kutish | `handle.join()` → blocking | `handle.await` → async |
| Resurs | OS thread (og'ir) | Async task (yengil) |
| Natija | `Result<T, JoinError>` | `Result<T, JoinError>` |

## Syntax and Examples

### Oddiy task spawning

```rust
fn main() {
    trpl::block_on(async {
        let handle = trpl::spawn_task(async {
            for i in 1..10 {
                println!("first task: {i}");
                trpl::sleep(Duration::from_millis(500)).await;
            }
        });

        for i in 1..5 {
            println!("main task: {i}");
            trpl::sleep(Duration::from_millis(500)).await;
        }

        handle.await.unwrap();  // birinchi task tugashini kut
    });
}
```

### Task handle — future

Task handle o'zi `Future` implement qiladi, shuning uchun `.await` bilan kutiladi. Natija `Result` — task panic bo'lganda `Err` qaytaradi.

```rust
let handle = trpl::spawn_task(async { 42 });
let result = handle.await.unwrap(); // result = 42
```

### Handle.await bo'lmasa nima bo'ladi?

`main` async bloki tugaganda unga biriktirilmagan barcha spawned task'lar o'chadi:

```rust
trpl::block_on(async {
    trpl::spawn_task(async {
        for i in 1..10 {
            println!("{i}");               // 1-2 dan keyin to'xtashi mumkin
            trpl::sleep(500ms).await;
        }
    });
    // handle yo'q — main blok tugaganda task ham to'xtaydi
    trpl::sleep(Duration::from_millis(100)).await;
});
```

## Common Mistakes

- **`handle.await` unutish**: main blok tugaganda spawned task ham o'chadi.
- **Task panic'ini e'tiborsiz qoldirish**: `handle.await` `Result` qaytaradi — `unwrap()` yoki proper error handling kerak.
- **Thread'dan farqni bilmaslik**: `spawn_task` OS thread yaratmaydi; agar task CPU-bound ish qilsa, runtime boshqa task'larni to'sib qo'yishi mumkin. CPU-bound uchun `spawn_blocking` ishlatiladi.

## Related Concepts

- [[future|Future trait]] — task handle future implement qiladi
- [[executor]] — tasklarni queue'dan olib poll qiladi
- [[waker|Waker]] — pending taskni qayta queue'ga yuboradi
- [[async-join|async join]] — bir nechta task/future'ni birgalikda kutish
- [[async-runtime|async runtime]] — task'larni boshqaruvchi executor
- [[threads]] — OS-darajasidagi og'ir ekvivalent
- [[join-handle]] — thread join handle ekvivalenti

## Sources

- [[wiki/sources/17-2-applying-concurrency-with-async]]
- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
