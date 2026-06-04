# Research Index — Quick Access
**Оновлено:** 2026-06-04  
**Читай цей файл першим коли починаєш нову сесію**

---

## Де що шукати

### Вибір продукту
→ [`products/ideas-catalog.md`](products/ideas-catalog.md)  
10 ідей з wow-фактором та часом збірки, топ-3 детально з demo script, адаптація під SKELAR групи, 5 критеріїв вибору

### Стартер шаблони та швидкий старт
→ [`code/starter-templates.md`](code/starter-templates.md)  
Топ GitHub templates для Expo+NativeWind+Supabase, команди для клонування, структура файлів, setup NativeWind v4

### Анімації (copy-paste, перевірений код)
→ [`code/animations.md`](code/animations.md)  
6 анімацій: Checkbox Bounce (5хв), Slide-in List (10хв), Swipe-to-Delete (20хв), Progress Ring (15хв), Streak Fire (10хв), Bottom Sheet (25хв)

### Core patterns (AsyncStorage, Zustand, Supabase Realtime, Sharing)
→ [`code/core-patterns.md`](code/core-patterns.md)  
AsyncStorage helpers, типи, streak calculator, HabitGrid, FlashList, Zustand store, Supabase realtime subscription, Expo Sharing, Haptics

### UI дизайн та AI slop checklist
→ [`design/ui-patterns.md`](design/ui-patterns.md)  
Кольори, spacing, typography, TaskRow анатомія, Empty State, FAB, Bottom Tabs, AI slop checklist

### День буткемпу — погодинний план
→ [`strategy/bootcamp-day-plan.md`](strategy/bootcamp-day-plan.md)  
8-годинний графік, scope decision rules, backup strategies, demo script template, Slack post template

### Claude Code + Expo налаштування
→ [`strategy/claude-code-expo-setup.md`](strategy/claude-code-expo-setup.md)  
CLAUDE.md шаблон для Expo, типові помилки моделі і як запобігти, паралельні агенти під час буткемпу

### Інфраструктура Claude Code (від Gemini Round 1)
→ [`gemini-bootcamp-infrastructure.md`](gemini-bootcamp-infrastructure.md)  
MCP setup, worktrees, memory systems, skills structure, competitor group map

### Expo MVP глибокий аналіз (від Gemini Round 2) ⭐ НОВИЙ
→ [`gemini-expo-mvp-deep.md`](gemini-expo-mvp-deep.md)  
SecureStore auth (не AsyncStorage!), 'use dom' директива, детальна карта 8 помилок,  
Callstack agent-skills, погодинний план Gemini, що різати

### Supabase: схеми, RLS, міграції ⭐ НОВИЙ
→ [`code/supabase-schemas.md`](code/supabase-schemas.md)  
SQL для habits/moods/wins, 5 RLS-патернів (owner/role/public/tenant/anon),  
типові помилки RLS, ER-діаграма, flow sequence, Supabase JS reference

### Zustand + FlashList + Reanimated — повний код ⭐ НОВИЙ
→ [`code/zustand-flashlist-complete.md`](code/zustand-flashlist-complete.md)  
Production Zustand store (persist+undo+optimistic), FlashList з анімаціями,  
топ-10 помилок Expo, FlashList vs FlatList таблиця, Jest тести

### Callstack agent-skills — повний каталог ⭐ НОВИЙ
→ [`code/callstack-agent-skills.md`](code/callstack-agent-skills.md)  
26 files з описами, 5 галюцинацій що запобігає, установка в Claude Code,  
які skills потрібні для буткемпу vs skip, expo/skills + devanshuDesai/agent-skills

### Winning Formula — корпоративний хакатон ⭐ НАЙВАЖЛИВІШИЙ
→ [`strategy/winning-formula.md`](strategy/winning-formula.md)  
3 critical success factors, Feature Vote Conversion Power table (9.8→8.1),  
Social Proof Moment technique, template comparison (toyamarodrigo=9.2 WINNER),  
'use dom' build times (Highcharts=1.5h fastest), 5-step winning strategy

### Що виграє хакатони 2026 + тренди + сумісність ⭐ НОВИЙ
→ [`strategy/hackathon-trends-2026.md`](strategy/hackathon-trends-2026.md)  
AI Agents топ-1, React Native Graph (Skia), нові бібліотеки, стек для перемоги,  
SDK 53 compatibility table, NativeWind 4.2.1 фіксі, viral demo patterns CEE

### Voice → Text транскрипція ⭐ НОВИЙ
→ [`code/voice-to-text.md`](code/voice-to-text.md)  
expo-speech-recognition (on-device, uk-UA) + Whisper API, повний код, flow для Meeting Extractor

### Charts — Skia/Victory ⭐ НОВИЙ
→ [`code/charts.md`](code/charts.md)  
react-native-graph (120fps line), Victory Native (bar), svg progress ring, коли що брати

### In-App Feedback (toast/banner) ⭐ НОВИЙ
→ [`code/in-app-feedback.md`](code/in-app-feedback.md)  
Toast компонент (Reanimated), демо-трюк "фейкове нагадування по таймеру"

### AI Features в Expo — готові патерни
→ [`code/ai-features.md`](code/ai-features.md)  
5 AI-патернів (Meeting Action Extractor #1), Claude direct call + Vercel AI SDK,  
streaming code, corporate vote ranking — "AI that ACTS" wins over "AI that summarizes"

### UX-психологія + наука демо-відео ⭐ НОВИЙ (ChatGPT #2)
→ [`strategy/ux-demo-psychology.md`](strategy/ux-demo-psychology.md)  
Сторітелінг структура, CTA правила, "10× момент", demo чекліст,  
скрипт з таймінгом 0:00-3:30, метрики успішності

### Промпти для інших AI
→ [`prompts-for-other-ais.md`](prompts-for-other-ais.md)  
ChatGPT #1 (Supabase schemas, errors, FlashList, Zustand), ChatGPT #2 (UX psychology),  
Grok #3 (trends), Grok #4 (competitors), Perplexity #5 (library compatibility)

### Deep Research Round 3 промпт ⭐ НОВИЙ
→ [`gemini-round3-prompt.md`](gemini-round3-prompt.md)  
Callstack agent-skills зміст, 'use dom' demo apps, що виграє корпоративні голосування, шаблони

---

## Quick Decision Tree

```
Отримав завдання буткемпу
  │
  ├─ Відкрий products/ideas-catalog.md
  │    └─ Вибери ідею → /office-hours → /spec
  │
  ├─ Клонуй шаблон з code/starter-templates.md
  │    └─ toyamarodrigo/expo-router-template (рекомендовано)
  │
  ├─ Відкрий strategy/bootcamp-day-plan.md
  │    └─ Слідуй погодинному плану
  │
  └─ Під час збірки:
       ├─ Анімації → code/animations.md
       ├─ Patterns → code/core-patterns.md
       └─ Дизайн → design/ui-patterns.md
```
