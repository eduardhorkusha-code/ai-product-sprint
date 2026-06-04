# MODULES — каталог-конструктор

> 🧱 Обираєш потрібні цеглинки → збираєш як єдиний продукт. Кожен модуль = готовий код.
> У день X: дивишся завдання → береш набір (нижче) або збираєш свій → copy-paste.

---

## 🧰 Повний каталог модулів

| # | Модуль | Що дає | Час | Демо-готовий | Файл |
|---|--------|--------|-----|--------------|------|
| **M1** | Realtime Pulse | анонімний збір → live-дашборд score-ring | 75хв | ✅ | [BACKUP-team-energy-pulse.md](BACKUP-team-energy-pulse.md) |
| **M2** | Onboarding Funnel | quiz → портрет → paywall (SKELAR-бізнес) | 30-40хв | ✅ | [../research/code/onboarding-funnel.md](../research/code/onboarding-funnel.md) |
| **M3** | Admin Dashboard | MRR/воронка/active (повний продукт) | 20-30хв | ✅ | [../research/code/admin-dashboard.md](../research/code/admin-dashboard.md) |
| **M4** | Payment Flow | paywall → підписка (mock на демо) | 10хв | ✅ | [../research/code/payment-flow.md](../research/code/payment-flow.md) |
| **M5** | Auth Screens | реєстрація/вхід (Supabase) | 20-30хв | 🟡 опц | [../research/code/auth-screens.md](../research/code/auth-screens.md) |
| **M6** | App Essentials | notifications/analytics/profile/permissions | 10-15хв кожна | ✅ | [../research/code/app-essentials.md](../research/code/app-essentials.md) |
| **M7** | AI Feature | Claude streaming, voice→text | 35-45хв | ✅ | [../research/code/ai-features.md](../research/code/ai-features.md) |
| **M8** | Charts | line(Skia)/bar/progress ring | 15-45хв | ✅ | [../research/code/charts.md](../research/code/charts.md) |
| **M9** | Animations | checkbox/streak-fire/swipe/sheet | <30хв кожна | ✅ | [../research/code/animations.md](../research/code/animations.md) |
| **M10** | Core Patterns | storage/Zustand/streak/FlashList/haptics/share | база | ✅ | [../research/code/core-patterns.md](../research/code/core-patterns.md) |
| **M11** | In-App Feedback | toast/banner (фейк push) | 10хв | ✅ | [../research/code/in-app-feedback.md](../research/code/in-app-feedback.md) |
| **M12** | Supabase Data | SQL+RLS+realtime, 5 патернів | база | ✅ | [../research/code/supabase-schemas.md](../research/code/supabase-schemas.md) |
| **M13** | UI System | кольори/spacing/AI slop checklist | база | ✅ | [../research/design/ui-patterns.md](../research/design/ui-patterns.md) |

**Спільне ядро:** M1/M3/M8 використовують ОДИН патерн (score-ring + LineGraph).
M2/M4 діляться paywall-екраном. M6 analytics живить M3 воронку реальними числами.

---

## 📦 Готові НАБОРИ (обери під тему завдання)

### Набір A — "Subscription B2C product" (повний SKELAR-вайб) ⭐
**Коли:** тема про продукт/підписку/персоналізацію/wellness.
```
M2 Funnel → M4 Payment → M1/dashboard → M3 Admin → M6 analytics
= voronka → портрет → paywall → робочий продукт → бізнес-метрики
```
Демо-наратив: user-flow → funnel → admin = "це бізнес, не фіча". Найсильніший під суддів.
Час: ~2.5-3 год (тісно, але це центр). Core first, admin якщо встигаєш.

### Набір B — "Live team tool" (realtime wow) ⭐
**Коли:** тема про команду/опитування/фідбек/настрій/енергію.
```
M1 Realtime Pulse (центр) → M9 анімації → M6 analytics → опц M3 admin
```
Демо: Social Proof Moment (аудиторія шле live). Найшвидший wow. Без auth.
Час: ~1.5-2 год. Найбезпечніший під 4h.

### Набір C — "AI productivity" 
**Коли:** тема про AI/автоматизацію/асистента.
```
M7 AI (voice→action) → M10 core → M9 анімації → M2 funnel(опц)
```
Демо: "AI that ACTS" (voice→структурована дія). Час: ~2 год.

### Мінімальний набір (страховка, ~1 год)
```
M10 core + M1 АБО простий CRUD + M9 одна анімація + seed data
```
Якщо все йде не так — один робочий екран з wow-анімацією > три недороблені.

---

## 🔧 Порядок збірки (будь-який набір)
```
1. SCAFFOLD: clone toyamarodrigo + STARTER.md (supabase.ts, metro, CLAUDE.md)
2. DATA: Supabase schema (з обраних модулів) — БД з prep вже стоїть
3. CORE: головний модуль набору end-to-end
4. ASSEMBLE: додаєш модулі-цеглинки по черзі (кожен незалежний)
5. POLISH: M9 анімації + M13 UI checklist
6. DEMO: M3 admin (якщо є) + Social Proof Moment
```

## Правило конструктора
- Кожен модуль НЕЗАЛЕЖНИЙ — додаєш/прибираєш без поломки решти.
- Беремо МІНІМУМ під wow. Зайвий модуль = розмитий фокус (Cut > Debate).
- "Повний продукт" відчуття дають: funnel + payment + admin (навіть mock). Дешево через реюз.
