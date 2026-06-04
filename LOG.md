# Sprint Log — ai-product-sprint

Хронологічний лог дій. Кожен новий запис — зверху. Читай першу секцію щоб знати де ми зараз.

---

## 🟢 ЗАРАЗ — де ми стоїмо

**Дата:** 2026-06-04 (буткемп 7 червня — 3 дні)  
**Статус:** База знань ПОВНА. 15+ research файлів. Всі промпти закриті (ChatGPT #1+#2, Grok, Perplexity, AI integration).  
**Наступний крок — SETUP (не ресерч):**
- [ ] Expo Go на iPhone + Expo Orbit на Mac
- [ ] Клонувати toyamarodrigo/expo-router-template, перевірити запуск
- [ ] Supabase проект (URL + ANON KEY)
- [ ] Callstack agent-skills в .claude/
- [ ] Протестувати SecureStore supabase.ts
- [ ] Figma: 3 UI kits в Drafts (вже зроблено)

**На буткемпі:** завдання → `research/INDEX.md` → вибрати продукт → `/office-hours`.

---

## Лог

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
