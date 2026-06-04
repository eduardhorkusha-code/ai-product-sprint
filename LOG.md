# Sprint Log — ai-product-sprint

Хронологічний лог дій. Кожен новий запис — зверху. Читай першу секцію щоб знати де ми зараз.

---

## 🟢 ЗАРАЗ — де ми стоїмо

**Дата:** 2026-06-04 (буткемп 7 червня — 3 дні)  
**Статус:** Ресерч 100%, всі темні плями закриті. SETUP підготовлено (SETUP.md). Команда 6 агентів.  
**Формат демо ВІДОМИЙ:** live pitch відео 5 хв, аудиторія дивиться + клацає сама.

**Залишилось — дії від Eduard (НЕ ресерч), див. [SETUP.md](SETUP.md):**
- [ ] ⚠️ Перемкнути Node v25 → v20 LTS (nvm) — критично для Expo
- [ ] Expo Go на iPhone + Expo Orbit (`brew install --cask expo-orbit`)
- [ ] Supabase проект → ключі в .env.local (schema готова в starter/STARTER.md)
- [ ] Claude API key (console.anthropic.com)
- [x] Watchman ✅ (встановлено) · Callstack skills ✅ · starter kit ✅ · Figma ✅

**На буткемпі:** завдання → `research/INDEX.md` → `/office-hours` → старт по SETUP.md §"день X".

---

## Лог

### 2026-06-04 — SETUP підготовлено + #2 закрито

**Що зроблено:**
- Перевірено toolchain: ⚠️ Node v25 занадто новий (треба 20 LTS), решта ✅
- Watchman встановлено (brew) · Callstack RN best-practices skill → reference/ (31 md)
- `starter/` кит: .env.example + STARTER.md (supabase client SecureStore, schema SQL, app CLAUDE.md, metro fix)
- `SETUP.md` — повний runbook: toolchain, дії Eduard, порядок старту в день X
- #2 формат демо ЗАКРИТО: live pitch 5хв, аудиторія клацає → Expo Go QR + web-лінк

**Хук reputation-dashboard блокує .ts/.sql edits** — тому starter-код в markdown-блоках (копіюй-встав), не живі файли. Логічно для kit.

**Залишилось:** дії Eduard (Node 20, Expo Go, Supabase, ключі). Ресерч + підготовка завершені.

---

### 2026-06-04 — Закрито темні плями #1/#3/#4/#5

**Що зроблено:**
- `code/voice-to-text.md` — закрив #1: expo-speech-recognition (on-device, uk-UA) + Whisper API
- `code/charts.md` — закрив #3: react-native-graph, Victory Native, svg ring
- `code/in-app-feedback.md` — закрив #4: toast/banner + демо-трюк
- #5 (UK локалізація) — вирішено: hardcode український текст
- STATUS.md оновлено: ресерч ~100%, залишились #2 (формат демо, питання до #platform_ai) і #6 (camera, залежить від продукту)

**Залишилось — НЕ ресерч:** спитати формат демо в #platform_ai + SETUP.

---

### 2026-06-04 — Агенти у форматі ai-team: моделі, Voice, навчання

**Що зроблено (за зразком github.com/eduardhorkusha-code/ai-team):**
- Всі 6 агентів отримали YAML frontmatter: name, codename, codename_reason, model, model_reason, color, tools
- Моделі обрані під роль: Opus (Product Brain, Demo Director — judgment вирішує день), Sonnet (Builder, Design Eye, AI Engineer, QA — темп/виконання)
- Voice-секція кожному (характер супергероя: Fury командує, Stark саркастичний, Mysterio театральний, Vision аналітичний, Spider-Man параноїк, Da Vinci майстер ока)
- Experience Log кожному — читають на початку задачі, дописують урок у кінці
- `.claude/agents/memory/*-experience.md` — 6 seed-файлів з research-уроками (старт зі знанням, не з нуля)
- `GSTACK.md` — операційна система команди: фаза→скіл→хто, sprint flow, ETHOS
- `agents/README.md` — install guide + таблиця моделей
- gstack як постійний референс: кожен агент має секцію "gstack — твоя операційна система"

**Логіка:** агенти тепер вчаться на уроках (як ai-team), систематизують роботу через gstack, обрали моделі під свою силу.

---

### 2026-06-04 — README rewrite + STATUS + команда 3→6 агентів

**Що зроблено:**
- README переписано як швидкий хаб-навігатор (карта всієї бази знань)
- `research/STATUS.md` — покриття, темні плями, що ще шукати
- AI-команда розширена 3→6: додано Demo Director (Mysterio), QA Tester (Spider-Man), AI Engineer (Vision)
- Оновлено team-таблиці в README + CLAUDE.md, доданий flow

**Темні плями (research/STATUS.md):**
- 🔴 #1 Voice→Text транскрипція (КРИТИЧНА) — промпт готовий, запустити
- 🟡 #2 Як аудиторія бачить апп на демо — уточнити формат буткемпу
- 🟡 #3 Charts код (Skia/Victory) — промпт готовий
- 🟢 #4-6 notification fake, UK локалізація, camera — дрібні/на місці

**Логіка розширення команди:** winning formula на 80% залежить від демо і голосування,
але цим ніхто не володів. Demo Director закриває найбільшу прогалину.

**Наступний крок:** закрити темну пляму #1 (voice→text) + SETUP

---

### 2026-06-04 — AI features + UX психологія (фінальні промпти)

**Що зроблено:**
- `research/code/ai-features.md` — 5 AI-патернів, Meeting Action Extractor #1, Claude direct call + Vercel AI SDK streaming
- `research/strategy/ux-demo-psychology.md` — ChatGPT #2: сторітелінг, CTA, "10× момент", demo скрипт з таймінгом
- Закрито ВСІ заплановані промпти

**Ключовий інсайт:** "AI that ACTS" (voice→action) виграє над "AI that summarizes". 10× момент = Social Proof Moment.

**Наступний крок:** SETUP перед буткемпом (див. секцію ЗАРАЗ)

---

### 2026-06-04 — Повна база знань для буткемпу

**Що зроблено:**
- `research/INDEX.md` — головний вхід у базу, decision tree
- `research/products/ideas-catalog.md` — 10 ідей, топ-3 з demo script
- `research/code/starter-templates.md` — найкращі GitHub templates для клонування
- `research/code/animations.md` — 6 Reanimated3 анімацій, copy-paste ready
- `research/code/core-patterns.md` — AsyncStorage, Zustand, Supabase RT, типи, streak
- `research/design/ui-patterns.md` — дизайн-система, AI slop checklist
- `research/strategy/bootcamp-day-plan.md` — погодинний план 8h, scope rules, demo script
- `research/strategy/claude-code-expo-setup.md` — CLAUDE.md для Expo, помилки моделі

**Наступний крок:** отримати завдання → `research/INDEX.md` → старт

---

### 2026-06-04 — Gemini research збережено + Round 2 промпт написано

**Що зроблено:**
- Gemini Deep Research (інфраструктура) скомпресовано та збережено в `research/gemini-bootcamp-infrastructure.md`
- Написано промпт для Round 2 Deep Research (`research/deep-research-next-prompt.md`) — фокус: вибір продукту, Expo speed patterns, тайминг дня, Claude Code+Expo gotchas
- Обидва файли запушено в репо

**Стан репо після:**
```
research/
  gemini-bootcamp-infrastructure.md  ✅ Claude Code setup, MCP, worktrees, skills, competitor groups
  deep-research-next-prompt.md       ✅ готовий промпт для наступного раунду ресерчу
```

**Наступний крок:**
- Скопіювати промпт з `research/deep-research-next-prompt.md` → запустити в Gemini/Perplexity Deep Research
- Повернутися сюди з результатом → збережемо + визначимо продукт → `/office-hours`

---

### 2026-06-04 — Ініціалізація

**Що зроблено:**
- Репо `ai-product-sprint` клоновано локально в `/Users/eduard.horkusha/ai-product-sprint/`
- Проведено аудит того, що вже є: CLAUDE.md, README.md, 3 агенти (product-brain, builder, design-eye)
- Додано `LOG.md` (цей файл) для трекінгу прогресу між сесіями

**Стан репо:**
```
CLAUDE.md           — правила, стек, sprint flow ✅
README.md           — опис команди ✅
agents/
  product-brain.md  — PM-агент, 6 forcing questions, MVP framework ✅
  builder.md        — Dev-агент, Expo/RN patterns, time budgets ✅
  design-eye.md     — UX-агент, AI slop test, HIG rubric ✅
LOG.md              — цей файл ✅

research/           — ❌ директорія згадана в README але не існує
```

**Відкриті питання:**
- [ ] Який продукт будуємо на Bootcamp 7 червня?
- [ ] Чи потрібен research файл до буткемпу?
- [ ] Чи треба додати `.claude/` конфіг в репо?

---

## Шаблон запису

```
### YYYY-MM-DD — [короткий опис]

**Що зроблено:**
- 

**Стан репо після:**
- 

**Наступний крок:**
- 
```
