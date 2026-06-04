# Winning Formula — Corporate Hackathon
**Джерело:** Gemini Round 3 PDF "Hackathon Stack Research & App Ideas"  
**Дата:** 2026-06-04  
**Це найважливіший стратегічний файл**

---

## 3 Критичних фактори успіху (research-backed)

### 1. Solve Workspace Frictions — не будуй окремий острів
Переможці **інтегруються в існуючі платформи** (Slack, Teams) замість вимагати нових логінів.  
→ Для нас: якщо Team Pulse — голосування через Slack-канал (не окремий акаунт).

### 2. Business Impact over Complex Architecture
Успішні команди **уникають складних ML моделей**, обирають прості рішення реальних проблем.  
→ Для нас: Supabase realtime простіше за websocket-server власного виробництва.

### 3. Shorten Time-to-Value
Переможці **доводять до core feature за секунди** після запуску.  
→ Для нас: перший екран = wow-moment. Жодного onboarding flow.

---

## Feature Vote Conversion Power (наукові дані)

| Ранг | Фіча | Аудиторія | Conversion /10 | Докази |
|------|------|-----------|---------------|--------|
| **1** | **Instant Onboarding & Action** | HR, Marketing, All | **9.8** | Переміщення core features на home tab ↑ conversion на 30%. Нуль setup screens. |
| **2** | **Live Real-time Collaboration** | PM, Legal | **9.2** | Real-time updates на мобільному під час демо = powerful "wow". Живий оновлення з web інтерфейсу. |
| **3** | **Contextual Slack Automation** | Finance, PM | **8.7** | Дозволяє запитувати доступ або планувати зустрічі прямо всередині існуючого workspace — економить години context switching. |
| **4** | **Interactive Visualization** | Finance, Marketing | **8.1** | Заміна статичних таблиць динамічними charts = professional + polished feel. |

**Висновок:** будуй у такому порядку пріоритетів.

---

## ⭐ Social Proof Moment — головна зброя демо

**Це техніка #1 що перетворює скептиків на виборців.**

> *"Send an emoji flag in the #hackathon channel right now,  
> and let's watch the mobile dashboard update live."*

**Як це працює:**
1. Під час демо просиш аудиторію відправити emoji в Slack
2. Supabase realtime миттєво оновлює мобільний екран
3. Кожен у залі бачить свій input на твоєму телефоні
4. Technology feels real — цінність доведена миттєво

**Технічна реалізація:** 30 рядків Supabase realtime subscription.  
Це не фіча — це **демо-техніка**. Готуй її окремо від фічей.

```tsx
// Суть реалізації (вже є в research/code/supabase-schemas.md)
// Таблиця moods з анонімним INSERT + realtime subscription
// Під час демо: "Всі шлють 🚀 в Slack → дивимось на екрані"
```

---

## Порівняльна таблиця шаблонів (фінальна)

| Шаблон | Ціна | SecureStore | Routing | CSS | Backend | Animation | Setup | **Score** |
|--------|------|-------------|---------|-----|---------|-----------|-------|-----------|
| **toyamarodrigo** ⭐ | FREE (MIT) | ❌ AsyncStorage | Expo Router v3 + TypedRoutes | NativeWind v4 | None (manual) | Reanimated | **5 хв** | **9.2/10** |
| NativeLaunch.dev | $79.99/рік | ✅ Supabase Adapter | Expo Router (System Tab Nav) | UniWind + HeroUI | Supabase DB+Auth | Reanimated | 10-15 хв | 8.5/10 |
| chvvkrishnakumar | FREE | ❌ AsyncStorage | Expo Router + Platform Behaviors | NativeWind v4 | None (frontend) | Reanimated + **@gorhom Bottom Sheet** | 5 хв | 7.5/10 |
| Obytes RN Template | FREE | ✅ Encrypted storage layers | Expo Router (System Tab Nav) | NativeWind v4 | Generic API | Reanimated | 15-20 хв | 7.0/10 |

### Висновок: **toyamarodrigo — переможець**
- Безкоштовний MIT license
- Точно відповідає нашому стеку (NativeWind v4 + Expo Router v3)
- 5 хвилин до першого екрану
- Zustand + TanStack Query вже налаштовані
- **ВАЖЛИВО:** NativeLaunch.dev перейшов на UniWind/HeroUI Native — NativeWind тепер тільки в paid Pro tier

### Єдиний мінус toyamarodrigo: немає SecureStore
```bash
# Додаємо вручну (вже є готовий код в gemini-expo-mvp-deep.md)
npx expo install expo-secure-store react-native-url-polyfill
# Потім використай код з research/code/supabase-schemas.md → SecureStore adapter
```

---

## 'use dom' Visualizations — час реалізації

| Проект | Технологія | Що показує | Час реалізації |
|--------|-----------|-----------|---------------|
| **Highcharts Financial** | `highcharts-react-official` | Real-time pie + line charts | **~1.5 год** ← НАЙШВИДШИЙ |
| InCommon Org Chart | D3 + d3-org-chart | Dynamic team tree mapping thousands of nodes | ~3 год |
| Seat Selection Engine | HTML5 Canvas API | Interactive seat map 1000-4000 nodes + zoom/pan | ~4 год |

**Для буткемпу:** Highcharts = 1.5 год для impressive interactive dashboard. Оптимальний вибір.

### Highcharts 'use dom' шаблон:

```tsx
// components/chart-webview.tsx
'use dom'
import Highcharts from 'highcharts'
import HighchartsReact from 'highcharts-react-official'

interface Props {
  data: { name: string; y: number }[]
  onSelect: (name: string) => void
}

export default function FinancialChart({ data, onSelect }: Props) {
  const options = {
    chart: { type: 'pie', backgroundColor: 'transparent' },
    title: { text: null },
    series: [{
      data,
      point: { events: { click: function() { onSelect(this.name) } } }
    }],
    credits: { enabled: false }
  }
  return <HighchartsReact highcharts={Highcharts} options={options} />
}
```

```tsx
// Нативний батьківський екран
import FinancialChart from '@/components/chart-webview'
import * as Haptics from 'expo-haptics'

<View style={{ width: '100%', height: 250 }}>  {/* ЗАВЖДИ явні розміри! */}
  <FinancialChart
    data={chartData}
    onSelect={async (name) => {
      await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)
      router.push(`/details?category=${encodeURIComponent(name)}`)
    }}
    dom={{ scrollEnabled: false }}
  />
</View>
```

---

## Фінальна стратегія (з PDF)

**5 кроків для перемоги на буткемпі 7 червня:**

1. **Init:** `toyamarodrigo/expo-router-template` → бун install → expo start
2. **Security:** Вручну додати SecureStore adapter для Supabase
3. **Agent config:** Підключити Callstack `react-native-best-practices` до CLAUDE.md
4. **'use dom':** Highcharts для interactive charts (~1.5 год) → offload calculations до WebView
5. **Social Proof:** Supabase realtime → invite audience to Slack during demo → live update на екрані

**Переможна формула демо:**
```
Instant value (секунди) + Live realtime + Social proof moment = WIN
```
