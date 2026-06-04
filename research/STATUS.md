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

## ✅ ЗАКРИТІ ПЛЯМИ (2026-06-04)

### ✅ #1 — Voice → Text транскрипція — ЗАКРИТО
→ [code/voice-to-text.md](code/voice-to-text.md)
2 шляхи: expo-speech-recognition (on-device, uk-UA, нуль backend, потребує dev build)
АБО Whisper API (працює в Expo Go, потребує OPENAI key). Повний код обох + flow для
Meeting Action Extractor. Українська працює в обох.

### ✅ #3 — Charts код (Skia/Victory) — ЗАКРИТО
→ [code/charts.md](code/charts.md)
react-native-graph (line, 120fps wow), Victory Native XL (bar), svg progress ring
(легкий, чистий Expo Go). Setup + повний код + коли що брати.

### ✅ #4 — Notification faking — ЗАКРИТО
→ [code/in-app-feedback.md](code/in-app-feedback.md)
Toast/banner компонент (Reanimated, без бібліотек) + демо-трюк "нагадування по таймеру".

### ✅ #5 — Українська локалізація — ВИРІШЕНО (рішення, не ресерч)
**Рішення:** hardcode український текст у демо. PDF радив hardcode мову пріоритетного
ринку. i18n зайвий для 1-day demo. Тексти одразу українською.

---

## 🌓 ЩО ЗАЛИШИЛОСЬ (не ресерч — зовнішнє/рішення на місці)

### ✅ #2 — Формат демо — ВИРІШЕНО (2026-06-04)
**Формат:** Live pitch відео 5 хв. Аудиторія дивиться + може сама поклацати застосунок.
**Наслідок:** апп має бути доступним для кліку → розповсюдження:
- **Expo Go QR** — повний нативний досвід (haptics, realtime) для відео-демо
- **Web-лінк** (`expo start --web` → deploy) — нуль інсталу, клік з браузера в Slack
- Деталі в [SETUP.md](../SETUP.md) §6.

### 🟢 #6 — Camera / Image picker — ВІДКЛАДЕНО (залежить від продукту)
**Не плям, а умовність** — потрібно тільки якщо обрана ідея має фото (receipt, avatar).
Рішення на /office-hours у день буткемпу. Якщо так — швидкий промпт (5 хв), expo-image-picker.

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

- **Покрито:** ~100% ресерчу. #1, #3, #4 закриті кодом, #5 рішенням.
- **Залишилось 2 пункти — НЕ ресерч:**
  - #2 формат демо — питання до #platform_ai (дія Eduard)
  - #6 camera — умовність, залежить від вибору продукту (рішення на /office-hours)
- **Головний ризик:** SETUP — середовище ще не налаштоване (див. чекліст нижче)

**Висновок:** ресерч-фаза ЗАВЕРШЕНА. Залишилось: (1) спитати формат у #platform_ai,
(2) SETUP. Більше шукати нічого — все є в research/.
