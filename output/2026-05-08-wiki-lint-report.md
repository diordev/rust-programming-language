# Rust Wiki Lint Report

Date: 2026-05-08
Run: 3 (full cleanup — barcha qarorlar bajarildi)

## Scope

`wiki/`, `raw/`, `output/`, `infranodus/` tekshirildi. `wiki/_templates/` lint hisobidan chiqarildi.

## Summary

Wiki to'liq sog'lom holatda. Barcha real muammolar bartaraf etildi.

| Tekshiruv | Natija |
|-----------|--------|
| Wiki sahifalari | **503** |
| `raw/` fayllar | **94** |
| Buzilgan wikilinks | **0** ✓ |
| Noaniq wikilinks | **0** ✓ |
| Orphan sahifalar | **0** ✓ |
| Frontmatter muammolari | **0** ✓ |
| Noto'g'ri `type`/`status` | **0** ✓ |
| `raw/` ↔ `sources/` qoplanmagan | **0** ✓ |

## Bajarilgan Fixlar

### Run 2 (avtomatik minor fix, 10 link)

- `[[drop-trait|...]]` → `[[drop|...]]` (6 fayl, 9 hodisa)
- `[[wiki/index.md]]` → `[[index]]` (4 source fayl)

### Run 3 (qolgan barcha muammolarni hal qilish)

#### 1. Yangi sahifalar (2 ta)

- **`wiki/concepts/reference-counting.md`** — yangi concept page. Mexanizm va mental model ([[rc-t|Rc<T>]] vs [[arc-t|Arc<T>]] taqqoslash, common mistakes). 3 ta `[[reference-counting]]` linki endi to'g'ri yechiladi.
- **`wiki/snippets/option-take.md`** — yangi snippet page. `Option::take()` metodining state pattern, sentinel almashtirish, va `Drop::drop` cleanup foydalanishlari. 1 ta `[[option-take]]` linki endi to'g'ri yechiladi.

#### 2. Retarget (1 ta)

- `wiki/sources/16-2-...message-passing.md` da `[[mpsc]]` chiqarildi va `[[channels]]` izohi kengaytirildi: "[[channels]] — `std::sync::mpsc` (multiple producer, single consumer)".

#### 3. Glossary dublikatlari o'chirildi (3 fayl)

- `wiki/glossary/closures.md` ✗ → mazmuni `wiki/concepts/closures.md` ichida
- `wiki/glossary/iterators.md` ✗ → mazmuni `wiki/concepts/iterators.md` ichida
- `wiki/glossary/testing.md` ✗ → mazmuni `wiki/concepts/testing.md` ichida

Sabab: glossary nusxalari faqat 1-2 satr edi, lekin Obsidian wikilink basename noaniqligini keltirib chiqarardi (105 hodisa). Concept versiyalari to'liq, glossary nusxalari ortiqcha.

#### 4. `wiki/index.md` yangilandi

- Glossary bo'limidan: `[[closures]]`, `[[iterators]]`, `[[testing]]` o'chirildi
- Concepts bo'limiga: `[[reference-counting|reference counting]]` qo'shildi
- Snippets bo'limi: "No snippet pages yet" → `[[option-take|Option::take()]]`

## Qolgan Eslatma — Real Muammo Emas

26 ta `chapters/<slug>.md` va `sources/<slug>.md` juftligi bir xil basename'ga ega (masalan `chapters/4-understanding-ownership.md` ↔ `sources/4-understanding-ownership.md`). Bu wiki dizaynidan kelib chiqadi: bob bo'yicha o'rganish (chapter) va manba xulosasi (source) — alohida sahifalar.

**Joriy holat:** Hech qaysi referens yalang'och basename ishlatmaydi — barcha linklar full path bilan (`[[chapters/...]]`, `[[sources/...]]`). **Real muammo yo'q.**

**Konvensiya:** Bu juftliklarga link qilganda har doim full path ishlatish kerak.

## Lint Skripti

Joyi: `/tmp/wiki_lint.py` (vaqtinchalik). Tekshiruvlar:

1. Buzilgan wikilinks — har `[[link]]` mavjud sahifaga ko'rsatadimi
2. Noaniq wikilinks — yalang'och basename, > 1 fayl
3. Frontmatter validatsiyasi (`title`, `status`, `created`, `updated`)
4. `type` va `status` qiymatlari (CLAUDE.md ro'yxati)
5. Bir xil basename'li fayllar — kelajakdagi xavf
6. Orphan sahifalar — hech qaerdan bog'lanmagan
7. `raw/` ↔ `wiki/sources/` qoplanish

Markdown jadvalda `\|` (escaped pipe) wikilink alias'ini to'g'ri tanish uchun regex tuzatildi.

## Action Items

Hech narsa qolmadi. ✓
