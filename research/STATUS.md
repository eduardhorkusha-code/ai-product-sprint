# Research Status — Покриття та темні плями
**Оновлено:** 2026-06-04 (буткемп 7 червня — 3 дні)

---

## ✅ Покрито повністю (не чіпати)

| Тема | Файл | Якість |
|------|------|--------|
| Claude Code інфраструктура | gemini-bootcamp-infrastructure | ✅ |
| Expo setup + помилки (18 errors total) | gemini-expo-mvp-deep, zustand-flashlist | ✅ |
| Анімації (6 готових) | code/animations | ✅ |
| Core patterns (storage, streak, haptics) | code/core-patterns | ✅ |
| Supabase SQL + RLS (5 патернів) | code/supabase-schemas | ✅ |
| Zustand store (повний, undo) | code/zustand-flashlist-complete | ✅ |
| FlashList + Reanimated | code/zustand-flashlist-complete | ✅ |
| Стартові шаблони | code/starter-templates | ✅ |
| Callstack agent-skills (26 файлів) | code/callstack-agent-skills | ✅ |
| AI features (5 патернів) | code/ai-features | ✅ |
| UI дизайн + AI slop | design/ui-patterns | ✅ |
| Продуктові ідеї (10) | products/ideas-catalog | ✅ |
| Стратегія перемоги | strategy/winning-formula | ✅ |
| Погодинний план | strategy/bootcamp-day-plan | ✅ |
| UX + demo психологія | strategy/ux-demo-psychology | ✅ |
| Тренди 2026 + сумісність | strategy/hackathon-trends-2026 | ✅ |
| Інструменти (Figma, v0, Orbit) | tools-ecosystem | ✅ |

---

## 🌓 ТЕМНІ ПЛЯМИ — що ще треба знайти

### 🔴 #1 — Voice → Text транскрипція (КРИТИЧНА)
**Проблема:** AI Meeting Action Extractor (фіча #1 для перемоги) потребує перетворення
голосу в текст. Маємо `expo-audio` для запису, але НЕ маємо як audio стає текстом.

**Питання без відповіді:**
- Whisper API (OpenAI) чи AssemblyAI чи on-device?
- Чи працює в Expo Go без dev build?
- Скільки коштує / яка затримка?
- Чи можна Claude напряму згодувати аудіо?

**Промпт для ChatGPT/Perplexity:**
```
I need voice-to-text transcription in an Expo SDK 53 app (works in Expo Go, no dev build).
Use case: record a 20-second voice note → transcribe → send text to Claude for action extraction.
Compare: OpenAI Whisper API vs AssemblyAI vs expo-speech-recognition vs on-device.
For each: setup code, cost, latency, Expo Go compatibility. Give complete working code
for the fastest path: expo-audio record → transcribe → return text string.
```

### 🟡 #2 — Як аудиторія БАЧИТЬ застосунок під час демо
**Проблема:** Social Proof Moment = аудиторія взаємодіє. Незрозуміла механіка показу.

**Питання:**
- Eduard демонструє свій екран (QuickTime/iPhone Mirroring) — аудиторія шле emoji в Slack? ✅ це є
- Чи треба щоб аудиторія ВІДКРИЛА застосунок? Тоді Expo Go QR на багато пристроїв?
- Який реальний flow голосування — відео в Slack чи live демо в залі?

**Дія:** уточнити формат буткемпу в #platform_ai (live pitch vs відео vs обидва).

### 🟡 #3 — Charts код (Skia / Victory Native)
**Проблема:** React Native Graph (Skia) рекомендований як топ-візуалізація, але код є
тільки для Highcharts 'use dom'. Немає готового Skia/Victory коду.

**Промпт:**
```
Give me complete, runnable code for 3 chart types in Expo SDK 53 using react-native-graph
(Skia) and victory-native: (1) animated line chart for weekly progress, (2) bar chart for
category breakdown, (3) progress ring. Each must work in Expo Go, TypeScript strict,
with NativeWind styling. Include the exact install commands.
```

### 🟢 #4 — Notification faking pattern
**Проблема:** Кажемо "fake push з in-app banner" але немає коду.

**Дія:** простий — згенерувати in-app toast/banner компонент. Можна зробити локально без ресерчу.

### 🟢 #5 — Українська локалізація
**Проблема:** SKELAR корпоративна аудиторія. Чи має застосунок бути українською?
Hardcode UK strings чи швидкий i18n?

**Рішення (ймовірно):** hardcode український текст у corporate-демо (PDF радив hardcode
мову пріоритетного ринку). Не потребує ресерчу — рішення продукту.

### 🟢 #6 — Camera / Image picker (якщо продукт потребує фото)
**Проблема:** Якщо ідея потребує фото (receipt scan, avatar) — немає патерну
expo-image-picker / expo-camera.

**Дія:** залежить від вибору продукту. Якщо обираємо щось з фото — швидкий промпт.

---

## 🔧 SETUP — дії перед буткемпом (НЕ ресерч)

```
[ ] Expo Go на iPhone (App Store)
[ ] Expo Orbit на Mac (expo.dev/orbit)
[ ] Node 20+, Bun, Claude Code CLI — перевірити версії
[ ] Клонувати toyamarodrigo/expo-router-template → bun install → expo start
    → переконатись що відкривається в Expo Go ✅
[ ] Supabase проект створено → EXPO_PUBLIC_SUPABASE_URL + ANON_KEY у .env.local
[ ] Callstack agent-skills в .claude/ + wire в CLAUDE.md
[ ] Протестувати SecureStore supabase.ts (код в code/supabase-schemas.md)
[ ] Figma: 3 UI kits в Drafts ✅ (зроблено)
[ ] ANTHROPIC_API_KEY готовий (для AI-фічі, EXPO_PUBLIC_ для hackathon)
```

---

## 📊 Підсумок

- **Покрито:** 95% — вся стратегія, код, дизайн, AI
- **Критична темна пляма:** Voice→Text транскрипція (#1) — запустити промпт
- **Решта плям:** дрібні, вирішуються на місці або залежать від вибору продукту
- **Головний ризик:** не ресерч, а SETUP — середовище ще не налаштоване

**Висновок:** ресерч-фаза майже завершена. Запустити 1 критичний промпт (voice→text),
далі — SETUP, не пошук.
