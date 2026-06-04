# Що виграє хакатони — Червень 2026
**Джерело:** Grok (Prompt #3) + Perplexity (Prompt #5)  
**Дата:** 2026-06-04

---

## Топ-категорії переможців хакатонів (червень 2026)

| Місце | Категорія | Приклад | Чому виграє |
|-------|-----------|---------|------------|
| 🥇 1 | **AI Agents / Multi-agent systems** | LORE (GitLab) — 8 agents + knowledge graph | Судді в захваті від "автономності", wow-демо |
| 🥈 2 | **AI + реальна проблема** (health, sustainability, devtools) | Security scanner agents | Impact + polished demo |
| 🥉 3 | **Mobile-first з AI** | Expo + Claude/Grok APIs | Красивий демо на телефоні, швидко будується |
| 4 | **AI-powered devtools** | Code review agents, doc bots | Технічна аудиторія |
| 5 | **Fullstack швидкий прототип** | Next.js + Supabase + AI | Vercel deploy за хвилини |

**Висновок:** перемагає AI + реальна проблема + polished demo. Чистий mobile без AI — важче. Mobile + AI integration = сильна комбінація.

---

## Нові/Актуальні бібліотеки Expo/RN (червень 2026)

| Бібліотека | Що дає | Wow-потенціал | Час інтеграції |
|-----------|--------|--------------|---------------|
| **React Native Graph** (Skia) | Line charts 120 FPS, gestures, cubic bezier | ⭐⭐⭐⭐⭐ для dashboards | ~45 хв |
| **expo-widgets** | iOS widgets + Live Activities на React | ⭐⭐⭐⭐⭐ найвищий wow на demo | ~2-3 год (skip для буткемпу) |
| **Reanimated 4** | Must-have smooth UI | ⭐⭐⭐⭐ | вже стандарт |
| **Nitro Modules** | Нативні модулі 10x швидше за старі | ⭐⭐⭐ (для VisionCamera v3) | складно для 1 дня |
| **uniwind** | Circular theme transitions, tvOS | ⭐⭐⭐ гарні анімації за 5 хв | ~5 хв |
| **react-native-enriched** | Rich text editor | ⭐⭐ для note apps | ~30 хв |
| **expo-maps** | Краща підтримка New Architecture | ⭐⭐ (якщо потрібні карти) | ~30 хв |
| **New Architecture (SDK 53+)** | Fabric + TurboModule, React 19 | Обов'язково вмикати | 0 хв (дефолт в SDK 53) |

### React Native Graph — найважливіша нова бібліотека

```bash
npx expo install react-native-graph react-native-reanimated react-native-gesture-handler
```

```tsx
import { LineGraph } from 'react-native-graph'

const points = last7Days.map((day, i) => ({
  date: new Date(day),
  value: completedCount[i]
}))

<LineGraph
  points={points}
  animated={true}
  color="#6366f1"
  style={{ width: '100%', height: 200 }}
  enablePanGesture={true}
  onPointSelected={(point) => console.log(point.value)}
/>
```

**Чому топ:** 120 FPS, виглядає як Bloomberg Terminal, 45 хвилин до повноцінного interactive chart.

---

## Рекомендований стек для перемоги (2026)

```
Expo SDK 53+ (New Architecture увімкнено)
  + Expo Router v3 (typedRoutes: true)
  + NativeWind v4.2.1+
  + Reanimated 4 + Gesture Handler
  + Supabase (auth + DB + realtime)
  + Zustand + TanStack Query
  + React Native Graph (Skia) для візуалізацій
  + Claude API / Vercel AI SDK для AI-фічей
```

---

## Сумісність бібліотек з Expo SDK 53 (Perplexity)

**SDK 53 = React Native 0.79 + React 19 + New Architecture за замовчуванням**

| Бібліотека | Статус | Дія |
|-----------|--------|-----|
| `react-native-reanimated` | ⚠️ Часткова (New Arch) | Встановити останню версію |
| `@shopify/flash-list` | ⚠️ Часткова (New Arch) | Встановити останню версію |
| `react-native-maps` 1.20.x | ✅ OK | Interop layer працює |
| `@stripe/react-native` ≥0.45.0 | ✅ OK | New Arch підтримка |
| `@supabase/supabase-js` | ⚠️ Проблеми | Metro ES Module exports |
| `@firebase/*` | ⚠️ Проблеми | Те саме |
| `expo-av` | ❌ Deprecated | Замінити на `expo-audio` + `expo-video` |
| `expo-background-fetch` | ❌ Deprecated | Замінити на `expo-background-task` |

### Фікси для SDK 53

```js
// metro.config.js — для Supabase/Firebase ES Module проблем
const config = getDefaultConfig(__dirname)
config.resolver.unstable_enablePackageExports = false  // ← додай це
module.exports = withNativeWind(config, { input: './app/globals.css' })
```

```json
// app.json — opt-out з New Architecture якщо є несумісність
{
  "expo": {
    "newArchEnabled": false
  }
}
```

### NativeWind v4 — критичне

```bash
# НЕ встановлюй 4.2.0 — зламана
npm install nativewind@4.2.1  # мінімальна робоча версія
```

**Відомі баги v4:**
- `ref` втрачається на компонентах → виправлено в 4.2.1
- Live CSS updates не завжди оновлюються → рестарт Metro
- Babel конфлікт при SDK 54 → виправлено в 4.2.1

### nanoid — статус

```
Версія: 5.1.5 (березень 2025)
Weekly downloads: 52.3M+
React Native: ✅ повна підтримка
Залежності: 0 (нативний JS)
Статус: Stable, production-ready
```

Жодних проблем з Expo SDK 53.

---

## Viral Demo Patterns — Eastern European Tech (Grok)

### Ключові кейси

**ElevenLabs (Польща) — абсолютний чемпіон:**
- Патерн: **"Mind-blowing feature drop"** — 15-60 сек демо одного неможливого фічера
- Приклад: Gibberlink — два AI-агенти переходять на "секретну мову" з чіпами/тонами
- Результат: мільйони views, waitlist explosion, без платної реклами
- Механіка: emotional surprise + практична цінність

### Таблиця патернів (застосовуй для Slack голосування)

| Патерн | Опис | Ефективність |
|--------|------|-------------|
| **Wow Tech Reveal** | Коротке демо одного "impossible" фічера | 🔥 Найвища |
| **Before/After** | Порівняння "старе vs нове" | ✅ Висока (довір'я) |
| **Utility + Surprise** | Вирішує біль + magic moment | ✅ Висока (B2B) |
| **Hackathon Demo** | Запустити і показати результат | ✅ Органічний reach |
| **Creator Seeding** | Доступ інфлюенсерам перед релізом | 💰 Потребує мережі |

### Правила вірусного демо для SKELAR буткемпу

1. **15-45 сек кліп** — zero fluff, surprise в перші 3 секунди
2. **Починай з болю**, не з фічі: "Ти щоранку..."
3. **WOW-момент першим** — не зберігай на кінець
4. **Технічна якість** — CEE аудиторія чутлива до "fake demo"
5. **Shareable** — скріншот або GIF що добре виглядає в Slack

### Специфіка для корпоративного Slack голосування

Корпоративна аудиторія голосує **не за красивий UI**, а за:
- "Я б використовував це **завтра** в роботі"
- "Це вирішує **мій** конкретний біль"
- "Виглядає як **реальний** продукт, не студентський проект"

→ **Seed data** — заповни демо-даними що схожі на реальні робочі сценарії SKELAR
→ **Corporate language** — "команда", "звіт", "задача" замість "user", "item", "entry"
