# Onboarding Funnel — портрет користувача → підписка
**Джерело:** RevenueCat, Noom/Headway teardowns, FunnelFox, adamlyttleapps skill
**Дата:** 2026-06-04
**Навіщо:** SKELAR будує саме subscription B2C apps з якісними воронками. Судді-будівники
ОДРАЗУ впізнають "ця людина розуміє наш бізнес". Це сильний демо-диференціатор + 10× момент.

---

## Як це роблять найкращі (анатомія)

| Апп | Що робить | Урок |
|-----|-----------|------|
| **Headway** (40M users) | Перше питання легке (вік) → momentum. Чергує питання з мотивацією/соц-пруфом. Ніколи не перевантажує. | Старт легкий, value по дорозі |
| **Noom** | До 113 екранів, 10-15хв. Прогресивний commitment. **Анімований графік прогнозу поки "будуємо твою програму"**. Paywall після емоц. інвестиції. | Персоналізація = цінність |
| **Duolingo** | Ціль ПЕРШОЮ ("яка твоя ціль?"). MAU→paid 4%→9% (2020-2025). | Goal-first |
| **Cal AI** | Швидкий візуальний quiz → миттєвий персональний результат | Швидкий "aha" |

**Бенчмарки:** web2app paywall конвертує ~6% (3× мобільного). Trial 17-32 дні
конвертує 45.7% (vs 26.8% у 3-7 днів).

---

## 7 принципів (психологія конверсії)
1. **Goal-first** — перший екран: "Яка твоя головна ціль?" Користувач з заявленою ціллю мотивованіший.
2. **Micro-commitments** — одне питання на екран, прогрес-бар. Кожна відповідь = інвестиція.
3. **Персоналізація = цінність** — кожне питання робить результат "тільки для тебе".
4. **"Building your plan" момент** — анімація поки система "будує профіль". Носить емоцію.
5. **Прогноз/проекція** — анімований графік "через 4 тижні твоя енергія +40%". Це 10× момент.
6. **Соц-пруф по дорозі** — відгуки, "1M людей вже з нами", зірки.
7. **Soft paywall після value** — спочатку показуєш персональний результат, ПОТІМ підписка. Anchor ціни + trial.

---

## 🎯 Наша воронка (енергетичний профіль, ~30-40хв білду)

```
1. Hook       "Підвищ свою енергію за 4 тижні" + CTA
2. Goal       "Яка твоя головна ціль?" (більше енергії / фокус / менше вигорання)
3. Quiz ×3-4  рівень енергії / сон / що виснажує / коли провал (1 питання/екран + прогрес)
4. Building   "Будуємо твій енергетичний профіль..." (анімація 2-3с)
5. Result     "Твій портрет" + анімований графік прогнозу (+40% через 4 тижні)
6. Paywall    trial 7 днів / anchor ціни / соц-пруф (БЕЗ реальної оплати на демо)
```

⏱️ **Час:** ~30-40хв. У 4h-спринті — ОПЦІЙНИЙ add якщо core готовий рано, АБО центр продукту
якщо тема = "збери портрет/персоналізація/підписка". Сигналить судді розуміння бізнесу.

---

## Код (копіюй у день X)

### `lib/onboarding.ts` — конфіг + store портрета
```typescript
import { create } from 'zustand'

export const QUESTIONS = [
  {
    id: 'goal', title: 'Яка твоя головна ціль?',
    options: [
      { key: 'energy', label: 'Більше енергії', emoji: '⚡' },
      { key: 'focus', label: 'Кращий фокус', emoji: '🎯' },
      { key: 'burnout', label: 'Менше вигорання', emoji: '🧘' },
    ],
  },
  {
    id: 'level', title: 'Як ти зараз почуваєшся більшість днів?',
    options: [
      { key: 'low', label: 'Виснажений', emoji: '😴' },
      { key: 'mid', label: 'То густо то пусто', emoji: '😐' },
      { key: 'high', label: 'В цілому ок', emoji: '🙂' },
    ],
  },
  {
    id: 'drain', title: 'Що найбільше виснажує?',
    options: [
      { key: 'sleep', label: 'Поганий сон', emoji: '🌙' },
      { key: 'meetings', label: 'Безкінечні зустрічі', emoji: '📅' },
      { key: 'food', label: 'Енергетичні провали по їжі', emoji: '🍩' },
    ],
  },
] as const

interface OnboardingState {
  answers: Record<string, string>
  setAnswer: (q: string, a: string) => void
}
export const useOnboarding = create<OnboardingState>((set) => ({
  answers: {},
  setAnswer: (q, a) => set((s) => ({ answers: { ...s.answers, [q]: a } })),
}))
```

### `components/QuizStep.tsx` — один екран питання (прогрес + опції)
```tsx
import { View, Text, Pressable } from 'react-native'
import Animated, { FadeInRight, FadeOutLeft } from 'react-native-reanimated'
import * as Haptics from 'expo-haptics'

export function QuizStep({ question, index, total, onSelect }: {
  question: typeof QUESTIONS[number]; index: number; total: number
  onSelect: (key: string) => void
}) {
  return (
    <Animated.View entering={FadeInRight} exiting={FadeOutLeft}
      style={{ flex: 1, padding: 24, backgroundColor: '#fff' }}>
      {/* прогрес-бар */}
      <View style={{ height: 6, backgroundColor: '#f3f4f6', borderRadius: 3, marginTop: 16 }}>
        <View style={{ height: 6, width: `${((index + 1) / total) * 100}%`,
          backgroundColor: '#6366f1', borderRadius: 3 }} />
      </View>
      <Text style={{ fontSize: 13, color: '#9ca3af', marginTop: 8 }}>
        Крок {index + 1} з {total}
      </Text>

      <Text style={{ fontSize: 26, fontWeight: '800', marginTop: 32, marginBottom: 24 }}>
        {question.title}
      </Text>

      {question.options.map((opt) => (
        <Pressable key={opt.key}
          onPress={() => { Haptics.selectionAsync(); onSelect(opt.key) }}
          style={{ flexDirection: 'row', alignItems: 'center', gap: 14, padding: 18,
            borderRadius: 16, borderWidth: 2, borderColor: '#e5e7eb', marginBottom: 12 }}>
          <Text style={{ fontSize: 28 }}>{opt.emoji}</Text>
          <Text style={{ fontSize: 17, fontWeight: '600' }}>{opt.label}</Text>
        </Pressable>
      ))}
    </Animated.View>
  )
}
```

### `components/BuildingProfile.tsx` — "будуємо профіль" (Noom-момент)
```tsx
import { useEffect, useState } from 'react'
import { View, Text, ActivityIndicator } from 'react-native'

const MESSAGES = [
  'Аналізуємо твої відповіді...',
  'Будуємо енергетичний профіль...',
  'Підбираємо персональні інсайти...',
]
export function BuildingProfile({ onDone }: { onDone: () => void }) {
  const [msg, setMsg] = useState(0)
  useEffect(() => {
    const i = setInterval(() => setMsg((m) => Math.min(m + 1, MESSAGES.length - 1)), 800)
    const t = setTimeout(onDone, 2600)
    return () => { clearInterval(i); clearTimeout(t) }
  }, [])
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center', backgroundColor: '#0b0b0f' }}>
      <ActivityIndicator size="large" color="#6366f1" />
      <Text style={{ color: '#fff', fontSize: 18, fontWeight: '600', marginTop: 24 }}>
        {MESSAGES[msg]}
      </Text>
    </View>
  )
}
```

### `app/result.tsx` — персональний портрет + прогноз (10× момент)
```tsx
import { View, Text } from 'react-native'
import { LineGraph } from 'react-native-graph'  // або svg
import { useOnboarding } from '@/lib/onboarding'

export default function Result() {
  const { answers } = useOnboarding()
  // прогноз: проста крива росту енергії на 4 тижні
  const points = [40, 52, 68, 80].map((v, i) => ({ date: new Date(2026, 5, 6 + i * 7), value: v }))

  return (
    <View style={{ flex: 1, backgroundColor: '#0b0b0f', padding: 24, justifyContent: 'center' }}>
      <Text style={{ color: '#9ca3af', fontSize: 15 }}>Твій енергетичний портрет</Text>
      <Text style={{ color: '#fff', fontSize: 28, fontWeight: '800', marginTop: 8 }}>
        Ціль: {answers.goal === 'energy' ? 'більше енергії' : answers.goal === 'focus' ? 'фокус' : 'менше вигорання'}
      </Text>
      <Text style={{ color: '#22c55e', fontSize: 18, fontWeight: '700', marginTop: 24 }}>
        Прогноз: +40% енергії за 4 тижні
      </Text>
      <View style={{ height: 200, marginTop: 16 }}>
        <LineGraph points={points} animated color="#22c55e" style={{ flex: 1 }} />
      </View>
    </View>
  )
}
```

### `app/paywall.tsx` — soft paywall (БЕЗ реальної оплати)
```tsx
import { View, Text, Pressable } from 'react-native'

export default function Paywall({ onContinue }: { onContinue: () => void }) {
  return (
    <View style={{ flex: 1, backgroundColor: '#fff', padding: 24, justifyContent: 'center' }}>
      <Text style={{ fontSize: 28, fontWeight: '800', textAlign: 'center' }}>
        Розблокуй свій план
      </Text>
      <Text style={{ fontSize: 16, color: '#6b7280', textAlign: 'center', marginTop: 8 }}>
        ⭐⭐⭐⭐⭐ 4.9 · 12 000+ менеджерів
      </Text>

      {/* anchor: річний vs місячний */}
      <View style={{ marginTop: 32, gap: 12 }}>
        <View style={{ borderWidth: 2, borderColor: '#6366f1', borderRadius: 16, padding: 18 }}>
          <Text style={{ fontWeight: '700', fontSize: 17 }}>Річний — 7 днів безкоштовно</Text>
          <Text style={{ color: '#6b7280', marginTop: 4 }}>₴199/міс → ₴83/міс (–58%)</Text>
        </View>
        <View style={{ borderWidth: 1, borderColor: '#e5e7eb', borderRadius: 16, padding: 18 }}>
          <Text style={{ fontWeight: '600', fontSize: 17 }}>Місячний</Text>
          <Text style={{ color: '#6b7280', marginTop: 4 }}>₴199/міс</Text>
        </View>
      </View>

      <Pressable onPress={onContinue}
        style={{ backgroundColor: '#6366f1', borderRadius: 16, padding: 18, marginTop: 24 }}>
        <Text style={{ color: '#fff', fontWeight: '700', fontSize: 17, textAlign: 'center' }}>
          Почати 7 днів безкоштовно
        </Text>
      </Pressable>
      <Text style={{ color: '#9ca3af', fontSize: 12, textAlign: 'center', marginTop: 12 }}>
        Скасуй будь-коли. Нагадаємо за 2 дні до кінця тріалу.
      </Text>
    </View>
  )
}
```

**Прод-версія платежу:** RevenueCat (`react-native-purchases`) — НЕ для демо. На демо
paywall — це екран без реальної транзакції (кнопка веде в апп).

---

## 🔀 Адаптація під будь-який продукт
Воронка генерична — міняєш тільки `QUESTIONS` + рамку результату:
- Habit tracker → "яку звичку будуєш?", прогноз стріку
- Finance → "яка фінансова ціль?", прогноз заощаджень
- Будь-який wellness → goal-first + проекція

**Демо-сила:** показати воронку = показати що ти розумієш unit-економіку SKELAR.
Згадай у пітчі: "збираємо портрет → персональний план → trial→subscription. Web2app
paywall конвертує ~6%, втричі більше за App Store."

## Референс
Готовий Claude-скіл: github.com/adamlyttleapps/claude-skill-app-onboarding-questionnaire
(моделює Headway/Headspace/Noom — можна підглянути структуру).
