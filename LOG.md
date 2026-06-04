# Sprint Log — ai-product-sprint

Хронологічний лог дій. Кожен новий запис — зверху. Читай першу секцію щоб знати де ми зараз.

---

## 🟢 ЗАРАЗ — де ми стоїмо

**Дата:** 2026-06-04 · ⏱️ **БУТКЕМП: субота 6 червня, 15:00–19:00 = 4 ГОДИНИ**  
**Статус:** Ресерч 100%, плями закриті, SETUP готовий, команда 6 агентів. База реструктурована під 4-годинні фази.  
**🎯 Головний документ дня:** [PLAYBOOK.md](PLAYBOOK.md) — хвилинний план + мій воркфлоу під час тиску.  
**Формат демо:** live pitch відео 5 хв, аудиторія дивиться + клацає сама.

**Залишилось — дії від Eduard (НЕ ресерч), див. [SETUP.md](SETUP.md):**
- [ ] ⚠️ Перемкнути Node v25 → v20 LTS (nvm) — критично для Expo
- [ ] Expo Go на iPhone + Expo Orbit (`brew install --cask expo-orbit`)
- [ ] 🎯 ЗАВТРА: Supabase проект → розгорнути БЕКАП-БАЗУ (starter/BACKUP-team-energy-pulse.md § Schema) → ключі в .env.local
- [ ] Claude API key (console.anthropic.com)
- [x] Watchman ✅ · Callstack skills ✅ · starter kit ✅ · Figma ✅ · BACKUP-скелет ✅

**На буткемпі:** завдання → `research/INDEX.md` → `/office-hours` → старт по SETUP.md §"день X".

---

## Лог

### 2026-06-04 — Прибирання: консистентність репо до останньої версії

**Що:** повний прохід на узгодженість.
- GitHub опис оновлено: "SKELAR × Claude Bootcamp — 4-год спринт: база знань + 6 AI-агентів + конструктор з 13 модулів..." + теги (expo, react-native, claude-code, hackathon, supabase)
- CLAUDE.md Sprint Flow: 8h → актуальний 4-годинний (15:00-19:00) + лінк PLAYBOOK/MODULES
- README карта коду: всі 15 модулів (були 7) + лінк MODULES + energy-thesis у стратегії
- Дати виправлено скрізь: 7→6 червня, 4 години (STATUS, winning-formula, ideas-catalog)
- INDEX: додано payment/auth/app-essentials у фазу BUILD
- Перевірено: 0 застарілих дат поза архівом, всі 15 code-файлів у README

---

### 2026-06-04 — Каталог-конструктор модулів + payment flow

**Інсайт Eduard:** треба готові кастомні модулі як конструктор — обрав, зібрав єдиний продукт.

- `starter/MODULES.md` — КАТАЛОГ: 13 модулів (M1-M13) з часом/депсами/лінками + 4 готові НАБОРИ
  (A: subscription B2C, B: live team tool, C: AI productivity, мінімальний). Порядок збірки.
- `research/code/payment-flow.md` — ⚠️ реальні IAP не працюють у Expo Go → демо МОК (Zustand isPro, ~10хв). Прод = RevenueCat (react-native-purchases-ui, dev build). web2app ~6%.
- `research/code/auth-screens.md` — email/pw Supabase, ~20-30хв. Опційний (anonymous краще під 4h).
- `research/code/app-essentials.md` — notifications(fake)/analytics(живить admin воронку)/profile/permission priming.

**Ключове:** M1/M3/M8 = один патерн (score-ring+LineGraph). M2/M4 діляться paywall. Реюз
робить "повний продукт" (funnel+payment+admin) дешевим. Конструктор → 4h стає "обрав і зібрав".

---

### 2026-06-04 — Admin dashboard + донавчання команди

**Q1 (готовність команди):** структурно готова, АЛЕ агенти були засіяні ДО останнього ресерчу.
Донавчив через experience logs (механізм навчання): Product Brain тепер ВЛАСНИК бізнес-моделі
(funnel+admin+метрики), Builder знає готові скелети для реюзу, Demo Director — бізнес-мову
для суддів, Design Eye — funnel/paywall/admin візуал.

**Q2 (admin-панель):** ТАК, але lightweight mock (~20-30хв, один екран). Реюз нашого ж
дашборду+charts. Сигналить суддям-будівникам "продукт з unit-економікою, не фіча".
Будувати ТІЛЬКИ якщо core+funnel готові рано АБО тема=admin/ops. Не красти центр демо.

- `research/code/admin-dashboard.md` — KPI (MRR/active/trial→paid/churn) + воронка + MRR-графік + 1 real live-число. Реалістичні mock-значення.
- 4 experience logs донавчено (product-brain, builder, demo-director, design-eye)

**Інсайт:** vote-дашборд + admin + funnel-result = ОДИН патерн (score-ring + LineGraph), три екрани. Реюз робить "повний продукт" дешевим.

---

### 2026-06-04 — Onboarding funnel (SKELAR-бізнес диференціатор)

**Інсайт Eduard:** SKELAR будує subscription B2C apps з воронками — судді впізнають "розуміє бізнес".

**Що:** дослідив як роблять найкращі (Headway 40M, Noom 113 екранів, Duolingo goal-first, Cal AI). 7 принципів: goal-first, micro-commitments, персоналізація=цінність, "building your plan" момент, прогноз-графік (10× момент), соц-пруф, soft paywall після value.

- `research/code/onboarding-funnel.md` — анатомія + психологія + ПОВНИЙ код під нас:
  quiz store, QuizStep (прогрес+опції), BuildingProfile (Noom-момент), Result (прогноз-графік), Paywall (anchor+trial, без реальної оплати → RevenueCat для prod)
- Енергетична рамка (під тезу організатора), але QUESTIONS swappable під будь-який продукт
- ~30-40хв білду: ОПЦІЙНИЙ add якщо core готовий рано, АБО центр якщо тема=портрет/підписка
- Бенчмарк для пітчу: web2app paywall ~6% (3× App Store)

---

### 2026-06-04 — Бекап-скелет Team Energy Pulse (розгортаємо завтра)

**Що:** повний готовий скелет найімовірнішого продукту — щоб не витратити ні хвилини в суботу.
- `starter/BACKUP-team-energy-pulse.md` — повний код (schema+seed, usePulse realtime хук, vote-екран, live-дашборд зі score-кільцем, Social Proof Moment) + prep-чеклист + гайд адаптації
- **Завтра (prep):** розгорнути Supabase з цією схемою → БД стоїть готова
- **Адаптивність:** патерн = анонімний realtime-збір → live-агрегат. Підходить під pulse/poll/feedback/check-in/Q&A/настрій. Якщо тема схожа → copy-paste, економить ~40хв. Якщо ні → скидається за 2хв.
- Вплетено в SETUP (prep §3), PLAYBOOK (SCAFFOLD), INDEX (фаза 1)

---

### 2026-06-04 — Аналіз лекції організатора "Енергія менеджера"

**Що:** прочитано презентацію C-level організатора буткемпу. Теза: "Personal energy = NEW GOLD".

**Сигнал — що він цінує:** вимірювання/дані (Whoop/Oura, трекери), звички/стріки (10k кроків, "сила малих кроків"), рефлексія/OKR життя, AI-асистент (pro-AI, сам пропонує промпт), ком'юніті/змагання.

**Висновок:** загострено топ-продукт → **Team Energy Pulse** (перетин: його теза "керівник=опора" + вимірювання + Social Proof Moment + 4h + широкий vote). Energy Journal #2.

- `research/strategy/organizer-energy-thesis.md` — картина світу + продуктові висновки + мова демо (БЕЗ його приватних медданих — етика)
- ideas-catalog 4h-ready загострено під енергію
- Demo-мова: "енергія", "in data we trust", "сила малих кроків", Whoop-style кільця

⚠️ Не пивотити ВСЕ під одного організатора — беремо перетин з широкою корисністю.

---

### 2026-06-04 — Реструктуризація під 4 ГОДИНИ (критична зміна)

**Зміна:** буткемп виявився 4-годинним (6 червня 15:00-19:00), не 8h. Адаптовано все.

**Що зроблено:**
- `PLAYBOOK.md` — хвилинний план 4 годин (DECIDE 30 / SCAFFOLD 30 / BUILD 90 / POLISH 30 / QA 30 / PITCH 30) + escape hatches
- **Мій (Claude) воркфлоу під тиском:** працюю переважно INLINE, агентів спавню рідко (холодний спавн дорогий при 4h). Спавн тільки якщо економить час (паралельні незалежні шматки >30хв).
- `research/INDEX.md` переписано як фазовий навігатор — файли згруповані за фазами 0-4, відкриваю тільки файл поточної фази
- Відпрацьовані промпти → `research/_archive/` (4 файли)
- Таймлайн оновлено скрізь: README, LOG, дата 6 червня, 4 години
- bootcamp-day-plan.md (8h) позначено застарілим → PLAYBOOK головний

**Принцип дня:** одна фіча, один wow, один екран. Все поза 4h = не існує.

---

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
