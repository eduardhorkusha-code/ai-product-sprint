# Bootcamp Day Plan — 8-Hour Schedule
**Оновлено:** 2026-06-04  
**Принцип:** Demo > Perfect. Working UI beats clean architecture.

---

## Погодинний план (8h intensive)

```
08:00  Session start — DON'T CODE YET
       └─ Product Brain: /office-hours → 6 forcing questions
       └─ Product Brain: /spec → 1 user story, max 5 bullets, cut list
       └─ Результат: повна ясність що будуємо ДО першого рядка коду

09:00  Builder: Scaffold + navigation
       └─ clone nearest starter template (з research/code/starter-templates.md)
       └─ Expo Go on phone — перевір що відкривається
       └─ Bottom tabs або single screen — встанови навігацію
       └─ Підключи шрифти (Inter), colors.ts, types.ts
       └─ Checkpoint: порожній апп з правильною структурою у Expo Go

10:00  Builder: Core feature #1 (найважливіший flow)
       └─ Тільки main screen — один flow end-to-end
       └─ AsyncStorage або Zustand store
       └─ Без анімацій поки — просто щоб працювало
       └─ Checkpoint: основний flow рунається

11:30  Builder: Core feature #2 (якщо є)
       └─ Якщо #1 зайняло більше часу — #2 SKIP або FAKE
       └─ "Fake the hard parts" — hardcode data якщо треба

12:30  ОБІД + Design Eye: /design-review pass #1
       └─ Скріншот кожного екрану
       └─ AI slop checklist (research/design/ui-patterns.md)
       └─ Top 3 фікси за impact — ТІЛЬКИ їх

13:00  Builder: Анімації + polish
       └─ Додати 1-2 анімації з research/code/animations.md
       └─ Haptic feedback на головних діях
       └─ Empty state
       └─ Checkpoint: виглядає як реальний апп

14:30  Design Eye: /design-review pass #2
       └─ Фінальний AI slop check
       └─ Typography consistency
       └─ Spacing audit

15:00  Builder: Demo prep
       └─ Заповнити seed data (3-5 реальних прикладів)
       └─ Прибрати placeholder text
       └─ Перевірити на реальному телефоні (не тільки simulator)
       └─ Записати backup відео (якщо live demo впаде)

15:30  Product Brain: Demo script
       └─ 60-90 секунд сценарій
       └─ Починати з болю ("Ти щоранку...")
       └─ Показати WOW-момент перший
       └─ Закінчити CTA ("Голосуй за нас у #platform_ai")

16:00  Rehearsal + Buffer
       └─ Запис demo video
       └─ Screenshots для Slack
       └─ /canary — перевірити що все стабільно
```

---

## Scope Decision Rules (Non-Negotiable)

| Ситуація | Рішення |
|----------|---------|
| Фіча займе > 2 годин | CUT |
| Потрібен OAuth / third-party API | CUT або FAKE |
| Push notifications | SKIP — fake з in-app banner |
| Calendar integration | SKIP — hardcode дати |
| Offline sync / conflict resolution | SKIP — AsyncStorage достатньо |
| Анімація займе > 30 хвилин | Використай бібліотеку або пропусти |
| Backend складніший ніж Supabase CRUD | SIMPLIFY |

---

## Backup Strategies

### Якщо відстаєш по часу
```
1. Cut scope до 1 core screen
2. Hardcode всі дані — не витрачай час на CRUD
3. Зосередься на WOW-анімації на головному екрані
4. Одна гарна demo flow краща ніж три недороблені
```

### Якщо Expo Go падає
```
1. Expo Go crash → expo-dev-client або запустити на simulator
2. Metro bundler завис → Ctrl+C, expo start --clear
3. NativeWind не аплається → restart Metro
4. RN Reanimated errors → перевір babel.config.js плагін
```

### Якщо реалтайм не встигаєш
```
Team Pulse без Supabase:
- useInterval(() => setVotes(mockVotes()), 3000) — fake polling
- Виглядає так само для аудиторії
```

---

## Demo Script Template

```
ВІДКРИВАЄШ АПОСТРОФ (≈5 сек):
"[Назва] — для тих, хто [PAIN]."

ПОКАЗУЄШ ПРОБЛЕМУ (≈10 сек):
"Кожного [ранку/тижня/місяця] ти [pain point].
Зазвичай це займає [X хвилин/годин]."

ДЕМОНСТРУЄШ РІШЕННЯ (≈30 сек):
"Ось що відбувається з [Назва]."
[ПОКАЗУЄШ WOW-МОМЕНТ ПЕРШИМ]
"[Тап/свайп/input] — і [результат]."

ПОКАЗУЄШ ДРУГУ ФІЧУ (≈15 сек, якщо є):
"А ось [streak / share / realtime]."

CTA (≈10 сек):
"За 1 день — [що збудував].
Голосуй за нас у #platform_ai."
```

---

## WOW-Моменти по продуктах (заготовлені)

| Продукт | WOW-момент | Коли показувати |
|---------|------------|-----------------|
| Habit Stack | Streak counter + fire pulse | Після першого тапу |
| Team Pulse | Live голоси з'являються в realtime | Попросити когось проголосувати під час демо |
| Win Journal | Share card → красива картка в Photos | В кінці |
| Focus Flow | Timer починається + screen dimmed | На початку |
| Daily Win | Confetti коли all 3 done | Фінал демо |

---

## Slack Post Template (після демо)

```
🎯 [Назва] — [1-речення опис]

Що побудували за день:
• [Фіча 1]
• [Фіча 2]  
• [WOW момент]

Стек: Expo + React Native + [AsyncStorage / Supabase]
Команда: Eduard + 3 AI агенти (PM, Dev, Design)

[Скріншот або GIF]

👆 Голосуй якщо це б використовував у роботі!
```
