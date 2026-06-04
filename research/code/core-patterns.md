# Core Code Patterns — Copy-Paste Ready
**Оновлено:** 2026-06-04  
**Правило:** перевірений код, без `...` placeholders, одразу рунається

---

## AsyncStorage — Local Persistence

```bash
npx expo install @react-native-async-storage/async-storage
```

```tsx
// lib/storage.ts
import AsyncStorage from '@react-native-async-storage/async-storage'

export const save = async (key: string, data: unknown): Promise<void> => {
  await AsyncStorage.setItem(key, JSON.stringify(data))
}

export const load = async <T>(key: string, fallback: T): Promise<T> => {
  try {
    const raw = await AsyncStorage.getItem(key)
    return raw ? (JSON.parse(raw) as T) : fallback
  } catch {
    return fallback
  }
}

export const remove = async (key: string): Promise<void> => {
  await AsyncStorage.removeItem(key)
}
```

**Використання:**
```tsx
const [tasks, setTasks] = useState<Task[]>([])

useEffect(() => {
  load<Task[]>('tasks', []).then(setTasks)
}, [])

const addTask = (task: Task) => {
  const updated = [...tasks, task]
  setTasks(updated)
  save('tasks', updated)
}
```

---

## Types — Shared Interfaces

```tsx
// lib/types.ts

export interface Task {
  id: string
  title: string
  done: boolean
  createdAt: string // ISO date string
}

export interface Habit {
  id: string
  name: string
  emoji: string
  color: string
  completedDates: string[] // ['2026-06-01', '2026-06-02', ...]
}

export interface WinEntry {
  id: string
  text: string
  date: string
  color: string
}

export interface MoodEntry {
  id: string
  emoji: string
  label: string
  score: number // 1-5
  date: string
}
```

---

## Streak Calculator

```tsx
// lib/streak.ts

export const getStreak = (completedDates: string[]): number => {
  if (completedDates.length === 0) return 0
  
  const dateSet = new Set(completedDates)
  let streak = 0
  const date = new Date()
  
  // Check today first, then go backwards
  while (true) {
    const dateStr = date.toISOString().split('T')[0]
    if (!dateSet.has(dateStr)) break
    streak++
    date.setDate(date.getDate() - 1)
  }
  
  return streak
}

export const isCompletedToday = (completedDates: string[]): boolean => {
  const today = new Date().toISOString().split('T')[0]
  return completedDates.includes(today)
}

export const getLast30Days = (): string[] => {
  const days: string[] = []
  const date = new Date()
  for (let i = 0; i < 30; i++) {
    days.unshift(date.toISOString().split('T')[0])
    date.setDate(date.getDate() - 1)
  }
  return days
}
```

---

## GitHub-style Calendar Grid (30 днів)

```tsx
// components/HabitGrid.tsx
import { View, Pressable } from 'react-native'
import { getLast30Days } from '@/lib/streak'

export function HabitGrid({ completedDates, onDayPress }: {
  completedDates: string[]
  onDayPress?: (date: string) => void
}) {
  const days = getLast30Days()
  const dateSet = new Set(completedDates)
  
  return (
    <View style={{ flexDirection: 'row', flexWrap: 'wrap', gap: 4 }}>
      {days.map((day) => {
        const done = dateSet.has(day)
        return (
          <Pressable
            key={day}
            onPress={() => onDayPress?.(day)}
            style={{
              width: 28, height: 28, borderRadius: 6,
              backgroundColor: done ? '#6366f1' : '#f3f4f6',
            }}
          />
        )
      })}
    </View>
  )
}
```

---

## FlashList (замість FlatList — 5x швидше)

```bash
npx expo install @shopify/flash-list
```

```tsx
import { FlashList } from '@shopify/flash-list'

export function TaskList({ tasks }: { tasks: Task[] }) {
  return (
    <FlashList
      data={tasks}
      estimatedItemSize={72}
      keyExtractor={(item) => item.id}
      renderItem={({ item, index }) => (
        <TaskRow task={item} index={index} />
      )}
      contentContainerStyle={{ padding: 16 }}
      ItemSeparatorComponent={() => <View style={{ height: 8 }} />}
      ListEmptyComponent={() => (
        <View style={{ alignItems: 'center', paddingTop: 60 }}>
          <Text style={{ fontSize: 40 }}>✅</Text>
          <Text style={{ marginTop: 12, color: '#9ca3af', fontSize: 16 }}>
            Add your first task →
          </Text>
        </View>
      )}
    />
  )
}
```

---

## Zustand State Management (без Redux)

```bash
npm install zustand
```

```tsx
// lib/store.ts
import { create } from 'zustand'
import { persist, createJSONStorage } from 'zustand/middleware'
import AsyncStorage from '@react-native-async-storage/async-storage'
import type { Task } from './types'
import { nanoid } from 'nanoid/non-secure'

interface TaskStore {
  tasks: Task[]
  addTask: (title: string) => void
  toggleTask: (id: string) => void
  deleteTask: (id: string) => void
}

export const useTaskStore = create<TaskStore>()(
  persist(
    (set) => ({
      tasks: [],
      addTask: (title) => set((state) => ({
        tasks: [...state.tasks, {
          id: nanoid(), title, done: false,
          createdAt: new Date().toISOString()
        }]
      })),
      toggleTask: (id) => set((state) => ({
        tasks: state.tasks.map(t => t.id === id ? { ...t, done: !t.done } : t)
      })),
      deleteTask: (id) => set((state) => ({
        tasks: state.tasks.filter(t => t.id !== id)
      })),
    }),
    {
      name: 'tasks-storage',
      storage: createJSONStorage(() => AsyncStorage),
    }
  )
)
```

**Використання в компоненті:**
```tsx
const { tasks, addTask, toggleTask, deleteTask } = useTaskStore()
```

---

## Supabase Realtime List (для Team Pulse / live features)

```bash
npx expo install @supabase/supabase-js @react-native-async-storage/async-storage
```

```tsx
// lib/supabase.ts
import { createClient } from '@supabase/supabase-js'
import AsyncStorage from '@react-native-async-storage/async-storage'

export const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL!,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!,
  { auth: { storage: AsyncStorage, autoRefreshToken: true, persistSession: true } }
)
```

```tsx
// Realtime subscription в компоненті
useEffect(() => {
  // Завантажити початкові дані
  supabase.from('votes').select('*').then(({ data }) => {
    if (data) setVotes(data)
  })
  
  // Підписатись на live updates
  const channel = supabase
    .channel('votes-channel')
    .on('postgres_changes', {
      event: '*', schema: 'public', table: 'votes'
    }, (payload) => {
      if (payload.eventType === 'INSERT') {
        setVotes(prev => [...prev, payload.new as Vote])
      }
    })
    .subscribe()
  
  return () => { supabase.removeChannel(channel) } // cleanup!
}, [])
```

---

## Expo Sharing — Share як картка

```bash
npx expo install expo-sharing expo-view-shot
```

```tsx
import ViewShot from 'react-native-view-shot'
import * as Sharing from 'expo-sharing'

const cardRef = useRef<ViewShot>(null)

const shareCard = async () => {
  const uri = await cardRef.current?.capture?.()
  if (uri) await Sharing.shareAsync(uri)
}

// В JSX:
<ViewShot ref={cardRef} options={{ format: 'jpg', quality: 0.95 }}>
  <WinCard text={winText} color={selectedColor} />
</ViewShot>
<Pressable onPress={shareCard}>
  <Text>Share 🎉</Text>
</Pressable>
```

---

## Haptic Feedback — Quick Reference

```bash
npx expo install expo-haptics
```

```tsx
import * as Haptics from 'expo-haptics'

// Легкий тап (checkbox, toggle)
Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)

// Середній (кнопка add)
Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)

// Важкий (delete, reset)
Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Heavy)

// Успіх (streak milestone, all done)
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)

// Помилка
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Error)
```

---

## nanoid для ID (без uuid залежності)

```bash
npm install nanoid
```

```tsx
import { nanoid } from 'nanoid/non-secure'
// non-secure = без crypto, працює в RN без проблем

const id = nanoid() // 'V1StGXR8_Z5jdHi6B-myT'
```
