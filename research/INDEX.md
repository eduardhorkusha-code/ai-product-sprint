# INDEX — навігатор бази знань

> ⏱️ **Буткемп: субота 6 червня, 15:00–19:00 = 4 ГОДИНИ.** Не 8. Все має вміститись.
> Спочатку читай [PLAYBOOK.md](../PLAYBOOK.md) — хвилинний план 4 годин.

Файли згруповані за фазами спринту. Відкривай той, що під поточну фазу.

---

## ⏱️ ФАЗА 0 — DECIDE (15:00–15:30)
Вибір продукту + жорсткий scope.

| Файл | Коли відкрити |
|------|--------------|
| [products/ideas-catalog.md](products/ideas-catalog.md) | Вибрати продукт. **Під 4h дивись "4h-ready" мітку** |
| [strategy/organizer-energy-thesis.md](strategy/organizer-energy-thesis.md) | 🎯 теза організатора "Енергія = NEW GOLD" — тюнінг продукту під неї |
| [strategy/winning-formula.md](strategy/winning-formula.md) | Feature Vote Conversion, що будувати першим |

**Рішення за замовчуванням під 4h:** найпростіший high-wow продукт з вбудованим
Social Proof Moment. Кандидат №1 — **Team Pulse** (realtime emoji, без auth, wow = live).

---

## ⏱️ ФАЗА 1 — SCAFFOLD (15:30–16:00)
Шаблон запущено в Expo Go, БД готова.

| Файл | Що беру |
|------|---------|
| [../starter/BACKUP-team-energy-pulse.md](../starter/BACKUP-team-energy-pulse.md) | 🎯 ГОТОВИЙ СКЕЛЕТ — якщо тема схожа, клацаєш copy-paste. БД розгорнута завтра |
| [code/starter-templates.md](code/starter-templates.md) | Клон toyamarodrigo + NativeWind setup |
| [../starter/STARTER.md](../starter/STARTER.md) | Supabase клієнт, schema SQL, app CLAUDE.md, metro fix |
| [../SETUP.md](../SETUP.md) | Порядок старту (§"день X") |

---

## ⏱️ ФАЗА 2 — BUILD CORE (16:00–17:30)
Одна фіча end-to-end + wow-механіка.

| Файл | Для чого |
|------|----------|
| [code/core-patterns.md](code/core-patterns.md) | AsyncStorage, Zustand, streak, FlashList, Haptics, Sharing |
| [code/zustand-flashlist-complete.md](code/zustand-flashlist-complete.md) | Повний store (undo) + FlashList + Top-10 Expo errors |
| [code/supabase-schemas.md](code/supabase-schemas.md) | SQL + RLS + realtime (Social Proof Moment) |
| [code/ai-features.md](code/ai-features.md) | AI-фіча (Claude streaming), якщо обрали AI |
| [code/voice-to-text.md](code/voice-to-text.md) | Voice→текст (on-device або Whisper) |

---

## ⏱️ ФАЗА 3 — POLISH (17:30–18:00)
Анімації, дизайн, seed data.

| Файл | Для чого |
|------|----------|
| [code/animations.md](code/animations.md) | 6 анімацій (Checkbox→Streak Fire), кожна <30хв |
| [code/charts.md](code/charts.md) | react-native-graph / Victory / progress ring |
| [code/onboarding-funnel.md](code/onboarding-funnel.md) | 🎯 воронка quiz→портрет→paywall (SKELAR-бізнес!) повний код, ~30-40хв |
| [code/in-app-feedback.md](code/in-app-feedback.md) | Toast/banner, фейкове нагадування |
| [design/ui-patterns.md](design/ui-patterns.md) | Кольори, spacing, **AI slop checklist** |

---

## ⏱️ ФАЗА 4 — QA + PITCH (18:00–19:00)
Go/No-Go, відео 5хв, голоси.

| Файл | Для чого |
|------|----------|
| [strategy/ux-demo-psychology.md](strategy/ux-demo-psychology.md) | Demo скрипт, 10× момент, CTA |
| [strategy/winning-formula.md](strategy/winning-formula.md) | Social Proof Moment механіка |

---

## 📚 DEEP REFERENCE (читаю тільки якщо застряг)
| Файл | Що там |
|------|--------|
| [code/callstack-agent-skills.md](code/callstack-agent-skills.md) | RN performance, 5 галюцинацій що запобігає |
| [../reference/callstack-rn-best-practices/](../reference/callstack-rn-best-practices/) | 31 md: FPS, memory, lists, bundle |
| [strategy/claude-code-expo-setup.md](strategy/claude-code-expo-setup.md) | CLAUDE.md для Expo, помилки моделі |
| [strategy/hackathon-trends-2026.md](strategy/hackathon-trends-2026.md) | Тренди, бібліотеки, SDK 53 сумісність |
| [gemini-expo-mvp-deep.md](gemini-expo-mvp-deep.md) | SecureStore, 'use dom', карта 8 помилок Expo |
| [gemini-bootcamp-infrastructure.md](gemini-bootcamp-infrastructure.md) | MCP, worktrees, групи-конкуренти |
| [tools-ecosystem.md](tools-ecosystem.md) | Figma, v0, Orbit, Supabase MCP |
| [strategy/bootcamp-day-plan.md](strategy/bootcamp-day-plan.md) | ⚠️ 8h версія — ЗАСТАРІЛА, дивись PLAYBOOK.md |

Архів відпрацьованих промптів: [_archive/](_archive/)

---

## 🧭 Decision tree (під тиском часу)
```
Завдання отримано
  → PLAYBOOK.md (хвилинний план)
  → ФАЗА 0: ideas-catalog (4h-ready) → вибір → scope до 1 фічі
  → ФАЗА 1: starter-templates + STARTER.md → апп запущено
  → ФАЗА 2: core-patterns + supabase-schemas → фіча працює
  → ФАЗА 3: animations + ui-patterns → виглядає готово
  → ФАЗА 4: ux-demo-psychology → відео + Slack
```
