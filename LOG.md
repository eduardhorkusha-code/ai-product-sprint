# Sprint Log — ai-product-sprint

Хронологічний лог дій. Кожен новий запис — зверху. Читай першу секцію щоб знати де ми зараз.

---

## 🟢 ЗАРАЗ — де ми стоїмо

**Дата:** 2026-06-04  
**Статус:** Research Round 1 збережено. Чекаємо Deep Research Round 2.  
**Наступний крок:** Запустити промпт з `research/deep-research-next-prompt.md` → отримати результат → визначити продукт → `/office-hours`.

---

## Лог

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
