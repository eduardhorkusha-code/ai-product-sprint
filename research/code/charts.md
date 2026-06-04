# Charts — Skia / Victory Native (закриває темну пляму #3)
**Джерело:** WebSearch (margelo/react-native-graph, Victory Native XL, Expo Skia docs)
**Дата:** 2026-06-04

---

## TL;DR — що брати

| Бібліотека | Для чого | Wow | Setup |
|-----------|----------|-----|-------|
| **react-native-graph** (Skia) ⭐ | Line chart 120fps з gesture | ⭐⭐⭐⭐⭐ | середній |
| **Victory Native XL** | Bar/line/pie, гнучкий | ⭐⭐⭐⭐ | середній |
| **react-native-svg** (progress ring) | Простий ring/donut | ⭐⭐⭐ | легкий |

Всі через Skia + Reanimated + Gesture Handler. Для буткемпу: react-native-graph для
головного "wow" чарту, svg ring для streak/completion.

---

## Setup (спільний для Skia)

```bash
npx expo install @shopify/react-native-skia react-native-reanimated react-native-gesture-handler
# Skia працює з Expo SDK 53+
```

> Швидкий старт з нуля: `npx create-expo-app my-app -e with-skia` (преконфіг).

---

## 1. react-native-graph — Line Chart (головний wow) ⭐

```bash
npm install react-native-graph
```

```tsx
import { LineGraph } from 'react-native-graph'
import * as Haptics from 'expo-haptics'

const points = last7Days.map((day, i) => ({
  date: new Date(day),
  value: completedCount[i],
}))

<LineGraph
  points={points}
  animated={true}
  color="#6366f1"
  gradientFillColors={['#6366f180', '#6366f100']}
  enablePanGesture={true}
  onPointSelected={(p) => Haptics.selectionAsync()}
  onGestureStart={() => Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)}
  style={{ width: '100%', height: 200 }}
/>
```

**Wow:** 120fps, палець водиш по графіку → точки підсвічуються + haptic. Виглядає як Bloomberg.

---

## 2. Victory Native XL — Bar Chart (категорії)

```bash
npm install victory-native
# peer deps вже встановлені (skia, reanimated, gesture-handler)
```

```tsx
import { CartesianChart, Bar } from 'victory-native'
import { useFont } from '@shopify/react-native-skia'

const data = [
  { category: 'Search', value: 35 },
  { category: 'LLM', value: 30 },
  { category: 'Reviews', value: 20 },
  { category: 'Social', value: 15 },
]

<CartesianChart data={data} xKey="category" yKeys={['value']}>
  {({ points, chartBounds }) => (
    <Bar
      points={points.value}
      chartBounds={chartBounds}
      color="#6366f1"
      roundedCorners={{ topLeft: 6, topRight: 6 }}
      animate={{ type: 'timing', duration: 600 }}
    />
  )}
</CartesianChart>
```

---

## 3. Progress Ring (streak / completion) — svg, легкий

```bash
npx expo install react-native-svg
```

```tsx
import Animated, { useSharedValue, useAnimatedProps, withTiming } from 'react-native-reanimated'
import Svg, { Circle } from 'react-native-svg'

const AnimatedCircle = Animated.createAnimatedComponent(Circle)

export function ProgressRing({ progress, size = 80, color = '#6366f1' }: {
  progress: number; size?: number; color?: string
}) {
  const radius = (size - 8) / 2
  const circumference = 2 * Math.PI * radius
  const p = useSharedValue(0)

  useEffect(() => { p.value = withTiming(progress, { duration: 800 }) }, [progress])

  const animatedProps = useAnimatedProps(() => ({
    strokeDashoffset: circumference * (1 - p.value),
  }))

  return (
    <Svg width={size} height={size} style={{ transform: [{ rotate: '-90deg' }] }}>
      <Circle cx={size/2} cy={size/2} r={radius} stroke="#e5e7eb" strokeWidth={6} fill="none" />
      <AnimatedCircle cx={size/2} cy={size/2} r={radius} stroke={color} strokeWidth={6}
        fill="none" strokeDasharray={circumference} strokeLinecap="round"
        animatedProps={animatedProps} />
    </Svg>
  )
}
```

---

## Коли що
- **Dashboard / прогрес у часі** → react-native-graph (найбільший wow)
- **Категорії / порівняння** → Victory Native bar
- **Streak / % виконання** → svg progress ring (легкий, без Skia)
- **Складна інтерактивна візуалізація з web-бібліотек (D3/Highcharts)** → 'use dom'
  (research/gemini-expo-mvp-deep.md, Highcharts 1.5год)

**Примітка:** Skia-чарти потребують dev build на деяких налаштуваннях. svg ring працює
в чистому Expo Go. Для гарантії — тестувати на пристрої (QA Tester).
