# Zustand + FlashList + Reanimated — Повний паттерн
**Джерело:** ChatGPT (Prompt #1) — production-ready, TypeScript strict  
**Дата:** 2026-06-04

---

## Zustand Store — Habit Tracker (повний)

```typescript
// lib/store.ts
import { create } from 'zustand'
import { persist, createJSONStorage } from 'zustand/middleware'
import AsyncStorage from '@react-native-async-storage/async-storage'

interface Habit {
  id: string
  name: string
  emoji: string
  color: string
  isDone: boolean
  completedDates: string[]
}

interface HabitState {
  habits: Habit[]
  lastAction: { type: string; payload: any } | null

  // Дії
  addHabit: (habit: Habit) => void
  removeHabit: (id: string) => void
  toggleHabit: (id: string) => void
  markToday: (id: string) => void
  undo: () => void
}

export const useHabitStore = create<HabitState>()(
  persist(
    (set, get) => ({
      habits: [],
      lastAction: null,

      addHabit: (habit) => {
        // Оптимістичне оновлення — UI миттєво, потім Supabase
        const newHabits = [...get().habits, habit]
        set({ habits: newHabits, lastAction: { type: 'add', payload: habit } })
        // Далі: supabase.from('habits').insert(habit) → якщо помилка → undo()
      },

      removeHabit: (id) => {
        const removed = get().habits.find(h => h.id === id) || null
        set({
          habits: get().habits.filter(h => h.id !== id),
          lastAction: { type: 'remove', payload: removed },
        })
      },

      toggleHabit: (id) => {
        set((state) => {
          const newHabits = state.habits.map(h =>
            h.id === id ? { ...h, isDone: !h.isDone } : h
          )
          const toggled = newHabits.find(h => h.id === id)
          return { habits: newHabits, lastAction: { type: 'toggle', payload: toggled } }
        })
      },

      markToday: (id) => {
        const today = new Date().toISOString().split('T')[0]
        set((state) => ({
          habits: state.habits.map(h =>
            h.id === id && !h.completedDates.includes(today)
              ? { ...h, completedDates: [...h.completedDates, today], isDone: true }
              : h
          ),
          lastAction: { type: 'markToday', payload: { id, date: today } }
        }))
      },

      undo: () => {
        const action = get().lastAction
        if (!action) return

        if (action.type === 'add') {
          set({ habits: get().habits.filter(h => h.id !== action.payload.id) })
        }
        if (action.type === 'remove') {
          set({ habits: [...get().habits, action.payload] })
        }
        if (action.type === 'toggle') {
          set((state) => ({
            habits: state.habits.map(h =>
              h.id === action.payload.id ? { ...h, isDone: !h.isDone } : h
            )
          }))
        }
        set({ lastAction: null })
      },
    }),
    {
      name: 'habits-storage',
      storage: createJSONStorage(() => AsyncStorage),
      // ВАЖЛИВО: зберігаємо тільки habits, не lastAction
      partialize: (state) => ({ habits: state.habits }),
    }
  )
)
```

## Селектори (похідні дані)

```typescript
// Статистика для UI (денний підрахунок, %)
export const useHabitStats = () => {
  const habits = useHabitStore(state => state.habits)
  return {
    total: habits.length,
    done: habits.filter(h => h.isDone).length,
    percentDone: habits.length
      ? Math.round(habits.filter(h => h.isDone).length / habits.length * 100)
      : 0,
  }
}

// Streak для одної звички
export const useStreak = (habitId: string) => {
  const habit = useHabitStore(state => state.habits.find(h => h.id === habitId))
  if (!habit) return 0

  const dateSet = new Set(habit.completedDates)
  let streak = 0
  const date = new Date()

  while (true) {
    const dateStr = date.toISOString().split('T')[0]
    if (!dateSet.has(dateStr)) break
    streak++
    date.setDate(date.getDate() - 1)
  }
  return streak
}
```

---

## FlashList + Reanimated — Повний компонент

```typescript
// components/HabitList.tsx
import React, { useEffect } from 'react'
import { View, Text, Pressable } from 'react-native'
import { FlashList } from '@shopify/flash-list'
import { GestureHandlerRootView } from 'react-native-gesture-handler'
import Animated, {
  useSharedValue, FadeInLeft, FadeOutRight, Layout
} from 'react-native-reanimated'
import * as Haptics from 'expo-haptics'
import { useHabitStore } from '@/lib/store'

// ─── Item component ───────────────────────────────────────────────────────────
const HabitItem = ({ habit }: { habit: Habit }) => {
  const { toggleHabit } = useHabitStore()
  // КРИТИЧНО: скидати opacity при рециклізації (item.id змінився)
  const opacity = useSharedValue(0)

  useEffect(() => {
    opacity.value = 1  // fade-in при монтажі
  }, [habit.id])       // ← [habit.id] — не [], щоб скидало при recycling

  return (
    <Animated.View
      entering={FadeInLeft.duration(250)}
      exiting={FadeOutRight.duration(200)}
      layout={Layout.springify()}
      style={{ opacity: opacity.value }}
    >
      <Pressable
        onPress={() => {
          toggleHabit(habit.id)
          Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)
        }}
        style={{
          flexDirection: 'row', alignItems: 'center',
          padding: 16, gap: 12,
          backgroundColor: 'white', borderRadius: 12,
          marginBottom: 8,
        }}
      >
        {/* Checkbox */}
        <View style={{
          width: 28, height: 28, borderRadius: 14,
          borderWidth: 2, borderColor: habit.color,
          backgroundColor: habit.isDone ? habit.color : 'transparent',
          alignItems: 'center', justifyContent: 'center',
        }}>
          {habit.isDone && <Text style={{ color: 'white', fontSize: 14 }}>✓</Text>}
        </View>

        {/* Emoji + Name */}
        <Text style={{ fontSize: 20 }}>{habit.emoji}</Text>
        <Text style={[
          { flex: 1, fontSize: 17, color: '#111827' },
          habit.isDone && { textDecorationLine: 'line-through', color: '#9ca3af' }
        ]}>
          {habit.name}
        </Text>
      </Pressable>
    </Animated.View>
  )
}

// ─── List component ───────────────────────────────────────────────────────────
export const HabitList = () => {
  const habits = useHabitStore(state => state.habits)

  return (
    // GestureHandlerRootView ОБОВ'ЯЗКОВИЙ — інакше свайпи не працюють
    <GestureHandlerRootView style={{ flex: 1 }}>
      <FlashList
        data={habits}
        renderItem={({ item }) => <HabitItem habit={item} />}
        estimatedItemSize={72}      // ← ОБОВ'ЯЗКОВИЙ для FlashList
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ padding: 16 }}
        // Для гетерогенних елементів (різні висоти)
        // getItemType={(item) => item.type}
        ListEmptyComponent={() => (
          <View style={{ alignItems: 'center', paddingTop: 80 }}>
            <Text style={{ fontSize: 48 }}>🌱</Text>
            <Text style={{ fontSize: 20, fontWeight: '700', marginTop: 12 }}>
              No habits yet
            </Text>
            <Text style={{ color: '#6b7280', marginTop: 8 }}>
              Add your first habit to start building momentum
            </Text>
          </View>
        )}
      />
    </GestureHandlerRootView>
  )
}
```

---

## FlashList vs FlatList — Коли що

| Характеристика | FlashList | FlatList |
|---------------|-----------|----------|
| Рендеринг | RecyclerView (пул, re-use) | Virtualization (mount/unmount) |
| Продуктивність | ✅ Краща на слабких пристроях | ⚠️ Може гальмувати при анімаціях |
| `estimatedItemSize` | ✅ Обов'язковий (покращує точність) | ❌ Немає |
| Reanimated Layout | ✅ Повна підтримка | ⚠️ Ручний `LayoutAnimation` |
| Гетерогенні елементи | `getItemType` | `getItemLayout` |
| Пам'ять | ✅ Постійний пул (менше GC) | ⚠️ Більше GC при перевантаженні |

**Правило:** для буткемпу — завжди FlashList.

---

## Топ-10 помилок Expo (ChatGPT)

| Помилка | Кроки відтворення | Причина | Рішення |
|---------|------------------|---------|---------|
| **RN version mismatch** | `expo start` після оновлення SDK | Версії RN у JS-бандлі та нативній частині не збігаються | `rm -rf node_modules && npm install && npx expo-doctor --fix && npx expo start --clear` |
| **[runtime not ready] require** | `expo start` з Hermes (SDK 53+) | `require` не знайдено у Hermes | У `metro.config.js`: `config.resolver.unstable_enablePackageExports = false` |
| **ExpoModulesCore (Pods)** | `cd ios && pod install` | CocoaPods не знаходить specs ExpoModulesCore | `npx install-expo-modules` або `expo prebuild` |
| **EAS build: "install dependencies" fail** | `eas build --platform android` | Конфлікт peer-залежностей (RN-Firebase версій) | Оновити до сумісних версій React 18.1.0, видалити `node_modules`, `expo doctor --fix` |
| **"Welcome to Expo" зависає** | Запуск проекту (Expo Go, SDK 51+) | Конфлікт Babel-плагіну `react-native-dotenv` | Видалити плагін, залишити `presets: ['babel-preset-expo']`, `npx expo start -c` |
| **Expo Dev Client timeout** | `npx expo start --dev-client` | Dev Client не може підключитись до Metro | `npx expo start --dev-client --tunnel` або перевірити IP |
| **Помилки build (CocoaPods)** | `eas build` або `expo run:ios` | Несумісні pod-залежності або версія Xcode/Pods | `cd ios && pod repo update && pod install` |
| **module not found (expo-router, reanimated)** | Після оновлення SDK | Застарілі/конфліктні пакети | `expo install react-native-reanimated` (версії по гайдлайнам Expo) |
| **Metro Bundler не стартує** | `expo start`, немає реакції | Конфлікт портів або watchman cache | `expo start -c` або видалити кеш Metro і restart |
| **Інші помилки OTA, права** | `expo publish` або OTA | `expo publish` пропускає реліз | `config.plugins` для Monorepo, `expo-cli` версії відповідності |

**Головне правило при будь-якій помилці:**
```bash
# Nuclear option — вирішує 80% проблем
rm -rf node_modules .expo ios/Pods
bun install
npx expo doctor --fix
npx expo start --clear
```

---

## Порівняння сховищ стану

| Бібліотека | Тип | Плюси | Мінуси | Коли використовувати |
|-----------|-----|-------|--------|---------------------|
| **AsyncStorage** | Ключ-значення | Просто, крос-платформ | Повільніший для великих даних | Буткемп — default |
| **MMKV** | Ключ-значення (апаратне шифрування) | Дуже швидкий | Доп. установка | Продакшн з великим станом |
| **SQLite** | Файлова БД | ACID, потужна для складних даних | Складна інтеграція | Offline-first зі складними запитами |
| **react-native-mmkv-storage** | Ключ-значення | Блискавично швидка кеш-бібліотека | Власний API | Маленький обсяг, кеш |

---

## Jest тести для Zustand store

```typescript
// __tests__/store.test.ts
import { useHabitStore } from '../lib/store'

describe('Habit Store', () => {
  beforeEach(() => {
    useHabitStore.setState({ habits: [], lastAction: null })
  })

  it('adds a habit', () => {
    const store = useHabitStore.getState()
    store.addHabit({ id: '1', name: 'Test', emoji: '✅', color: '#6366f1', isDone: false, completedDates: [] })
    const { habits, lastAction } = useHabitStore.getState()
    expect(habits.length).toBe(1)
    expect(habits[0].name).toBe('Test')
    expect(lastAction).toMatchObject({ type: 'add', payload: { id: '1' } })
  })

  it('toggles habit completion', () => {
    const store = useHabitStore.getState()
    store.addHabit({ id: '2', name: 'A', emoji: '💧', color: '#3b82f6', isDone: false, completedDates: [] })
    store.toggleHabit('2')
    expect(useHabitStore.getState().habits.find(h => h.id === '2')?.isDone).toBe(true)
  })

  it('undoes last action', () => {
    const store = useHabitStore.getState()
    store.addHabit({ id: '3', name: 'B', emoji: '📚', color: '#10b981', isDone: false, completedDates: [] })
    store.undo()
    expect(useHabitStore.getState().habits.length).toBe(0)
  })
})
```
