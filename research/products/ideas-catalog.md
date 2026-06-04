# Product Ideas Catalog — Bootcamp Ready
**Оновлено:** 2026-06-04  
**⏱️ РЕАЛЬНІСТЬ: 4 ГОДИНИ.** Core feature має вміститись у ~90 хв (решта: decide/scaffold/polish/pitch).

---

## 🎯 4h-READY — бери одне з цих (core ≤90хв)

> 💡 **Тюнінг під організатора:** C-level організатор має тезу "Енергія менеджера = NEW GOLD"
> (див. [strategy/organizer-energy-thesis.md](../strategy/organizer-energy-thesis.md)).
> Топ-ідеї загострені під неї — без втрати широкої корисності.

| Ранг | Ідея | Wow | Чому влазить у 4h | Social Proof Moment? |
|------|------|-----|-------------------|----------------------|
| 🥇 | **Team Energy Pulse** — менеджер бачить енергію команди live | 9 | Без auth, 1 екран, realtime = весь wow. ~75хв core. | ✅ ВБУДОВАНИЙ + резонує з тезою організатора |
| 🥈 | **Energy Journal** — рефлексія енергії + AI-інсайт + share | 9 | Text + AI call + share. ~70хв. | 🟡 share card + рефлексія = його теза |
| 🥉 | **Daily Win** — 3 задачі + confetti | 8 | CRUD + confetti при all-done. ~70хв. | 🟡 простота |

**Default під 4h: Team Energy Pulse** — перетин 4 факторів:
(1) Social Proof Moment вбудований, (2) realtime = wow, (3) без auth економить годину,
(4) **резонує з тезою організатора** ("керівник = точка опори" + вимірювання енергії).
Demo-рамка: "Енергія команди сьогодні" + Whoop-style score кільце.

⚠️ **НЕ під 4h** (потребують >90хв core): Habit Stack (streak grid), Meeting Digest
(voice→text+AI), Sprint Board, Skill Pulse. Бери тільки якщо обрали свідомо і ріжемо решту.

---

## Повна матриця ідей (час нижче = для 6-8h; під 4h дивись 4h-READY вище)

| # | Ідея | Wow-фактор /10 | Час збірки | Аудиторія | Унікальний кут |
|---|------|---------------|------------|-----------|----------------|
| 1 | **Focus Flow** — pomodoro + task queue | 7 | 4–5h | Всі | Живий прогрес-бар, звуки, streak |
| 2 | **Daily Win** — 3-задачі на день | 8 | 3–4h | PM, Ops, HR | Радикальна простота, motion |
| 3 | **Habit Stack** — ланцюжки звичок | 9 | 5–6h | HR, Coach | Візуальний ланцюжок + streak fire |
| 4 | **Meeting Digest** — нотатки зі зустрічей | 7 | 5–6h | PM, Legal, HR | Voice → bullet points |
| 5 | **Energy Log** — трекер енергії/настрою | 8 | 4–5h | Всі | Простий gesture input, графік |
| 6 | **Sprint Board** — мобільна kanban дошка | 6 | 6–7h | PM, Dev | Swipe-to-move, realtime |
| 7 | **Win Journal** — щоденник досягнень | 9 | 3–4h | Всі | Красиво, емоційно, share card |
| 8 | **Quick Budget** — витрати в 2 тапи | 8 | 4–5h | Finance, Ops | Swipe categories, тижневий графік |
| 9 | **Skill Pulse** — ріст навичок по тижнях | 7 | 5–6h | HR, L&D | Radar chart, check-in flow |
| 10 | **Team Pulse** — анонімний mood check | 9 | 4–5h | PM, HR | Emoji vote → живий дашборд |

---

## Топ-3 детально

### 🥇 #3 — Habit Stack
**Чому виграє:**
- Поняття "streak" — universally understood, emotionally powerful
- "Ланцюжок звичок" (habit stacking) — трендова методологія Atomic Habits
- Красива візуалізація за 30 хвилин коду

**Core feature (1 screen):**
```
Morning Stack: ☀️ → 💧 → 🧘 → 📚 → ✅
[кожна іконка анімується при тапі, streak +1, chain glows]
```

**MVP scope (строго):**
- [ ] 1 stack із 3-5 звичок
- [ ] Тап = done, анімація + haptic
- [ ] Streak counter з fire emoji
- [ ] Calendar grid (GitHub-style) за 30 днів
- [ ] LocalStorage, без auth

**Cut list:** push notifications / multiple stacks / social features / categories

**Demo script (60 сек):**
> "Зранку ти відкриваєш застосунок. Ось твій ранковий стек. Тапаєш — і кожна звичка відмічається з тактильним відгуком. Streak 23 дні. А ось твоя сітка за місяць — як GitHub contributions але для твого здоров'я."

---

### 🥈 #10 — Team Pulse
**Чому виграє:**
- Корпоративна аудиторія одразу бачить себе користувачем
- Realtime — WOW-момент коли голоси з'являються live
- Простий UX: 1 тап → emoji → done

**Core feature:**
```
"Як ти сьогодні?" → 😴 😐 🙂 😄 🚀
[вибрав → з'являється в загальному live pulse → анонімно]
```

**MVP scope:**
- [ ] Emoji vote screen (5 варіантів)
- [ ] Live результати (Supabase realtime)
- [ ] Анонімність — no auth needed (device ID)
- [ ] Простий bar chart результатів

**Cut list:** history / trends / admin panel / notifications

**Demo script:**
> "Менеджер починає stand-up. Всі відкрили застосунок. За 10 секунд — 8 голосів. 3 ракети, 3 посмішки, 2 нейтральні. Одразу видно тон команди без незручних питань."

---

### 🥉 #7 — Win Journal
**Чому виграє:**
- Емоційно резонує з будь-якою аудиторією
- Share card → вірусність під час демо
- Мінімальний backend (або взагалі без нього)

**Core feature:**
```
"Яка твоя перемога сьогодні?" 
→ [красива картка з градієнтом + текстом]
→ [Share як зображення]
```

**MVP scope:**
- [ ] Текстовий input з лімітом
- [ ] 3-5 кольорових тем для картки
- [ ] Список перемог (AsyncStorage)
- [ ] Share як screenshot (expo-sharing)

---

## Ідеї під конкретні задачі від SKELAR груп

| Група | Їх домен | Адаптована ідея |
|-------|----------|-----------------|
| Група 1 (HR) | Рекрутинг | **Candidate Quick Notes** — голосова нотатка + статус після дзвінка |
| Група 2 (PM) | Фідбек | **Feedback Pulse** — аналог Team Pulse для продукту |
| Група 3 (Ops) | Звітність | **Daily KPI Check** — 5 метрик, зелено/червоно, тренд |
| Група 4 (Marketing) | Контент | **Content Calendar** — простий drag-n-drop плануванням постів |
| Група 5 (Finance) | Бюджет | **Quick Budget** — витрати в 2 тапи |
| Група 6 (Legal) | Документи | **Contract Checklist** — чекліст пунктів контракту |

---

## Критерії вибору (застосовуй під час /office-hours)

1. **Чи є 1 wow-момент** що можна показати за 10 секунд?
2. **Чи буде це реально використовуватись** після демо? (корпоративна аудиторія голосує за "корисне")
3. **Чи є motion/animation** що вражає нетехнічну людину?
4. **Чи можна скоротити** до 1 core action якщо часу немає?
5. **Чи є shareable moment** — screenshot/відео що добре виглядає в Slack?
