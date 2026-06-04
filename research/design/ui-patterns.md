# UI Patterns & Design Inspirations
**Оновлено:** 2026-06-04  
**Мета:** не вигадувати дизайн під час буткемпу — мати готові рішення

---

## Топ-апки для натхнення (що красти)

| Апп | Що красти | Платформа |
|-----|-----------|-----------|
| **Things 3** | Spacing system, typography hierarchy, empty states | iOS |
| **Streaks** | Habit completion circles, color system, grid | iOS |
| **Forest** | Progress visualization, gamification feel | iOS/Android |
| **Notion Mobile** | Card layouts, minimal chrome | iOS/Android |
| **Fantastical** | Bottom tab styling, calendar grid | iOS |
| **Bear** | Typography-first, soft shadows | iOS |
| **Todoist** | Task item anatomy, priority indicators | iOS/Android |
| **Superlist** | Modern task UI, gesture feel | iOS |

---

## Базова дизайн-система (copy-paste)

### Кольори (Indigo theme — safe choice)

```tsx
// lib/colors.ts
export const colors = {
  primary: '#6366f1',      // indigo-500
  primaryLight: '#e0e7ff', // indigo-100
  primaryDark: '#4338ca',  // indigo-700
  
  success: '#10b981',  // emerald-500
  danger: '#ef4444',   // red-500
  warning: '#f59e0b',  // amber-500
  
  text: '#111827',        // gray-900
  textSecondary: '#6b7280', // gray-500
  textDisabled: '#d1d5db',  // gray-300
  
  background: '#ffffff',
  surface: '#f9fafb',  // gray-50
  border: '#e5e7eb',   // gray-200
  
  // Streak colors
  fire: '#f97316',  // orange-500
}

// 5 пресетів для карток Win Journal
export const cardGradients = [
  ['#6366f1', '#8b5cf6'], // indigo → violet
  ['#10b981', '#059669'], // emerald
  ['#f59e0b', '#ef4444'], // amber → red
  ['#3b82f6', '#06b6d4'], // blue → cyan
  ['#ec4899', '#8b5cf6'], // pink → violet
]
```

### Spacing (multiples of 4)

```tsx
export const spacing = {
  xs: 4, sm: 8, md: 12, base: 16, lg: 20, xl: 24, '2xl': 32, '3xl': 48
}
```

### Typography (3 sizes max)

```tsx
export const typography = {
  title: { fontSize: 22, fontWeight: '700' as const, fontFamily: 'Inter_700Bold' },
  body:  { fontSize: 17, fontWeight: '400' as const, fontFamily: 'Inter_400Regular' },
  label: { fontSize: 14, fontWeight: '600' as const, fontFamily: 'Inter_600SemiBold' },
  caption: { fontSize: 13, fontWeight: '400' as const, fontFamily: 'Inter_400Regular', color: '#6b7280' },
}
```

### Border radius

```tsx
export const radius = {
  sm: 8, md: 12, lg: 16, xl: 20, full: 9999
}
```

---

## Анатомія task item (перевірений паттерн)

```tsx
// components/TaskRow.tsx
export function TaskRow({ task, onToggle, onDelete }: TaskRowProps) {
  return (
    <View style={{
      flexDirection: 'row', alignItems: 'center',
      paddingHorizontal: 16, paddingVertical: 14,
      backgroundColor: 'white', borderRadius: 12,
      gap: 12,
    }}>
      <CheckButton done={task.done} onToggle={() => onToggle(task.id)} />
      
      <Text style={[
        { flex: 1, fontSize: 17, color: '#111827' },
        task.done && { textDecorationLine: 'line-through', color: '#9ca3af' }
      ]}>
        {task.title}
      </Text>
      
      {/* Priority dot */}
      {task.priority === 'high' && (
        <View style={{ width: 8, height: 8, borderRadius: 4, backgroundColor: '#ef4444' }} />
      )}
    </View>
  )
}
```

---

## Empty State (ніколи не "No items found")

```tsx
// Завжди специфічний + actionable
function EmptyTasks() {
  return (
    <View style={{ alignItems: 'center', paddingTop: 80, paddingHorizontal: 32 }}>
      <Text style={{ fontSize: 48, marginBottom: 16 }}>📋</Text>
      <Text style={{ fontSize: 20, fontWeight: '700', color: '#111827', textAlign: 'center' }}>
        Nothing here yet
      </Text>
      <Text style={{ fontSize: 16, color: '#6b7280', textAlign: 'center', marginTop: 8 }}>
        Add your first task and start building momentum
      </Text>
      <Pressable style={{
        marginTop: 24, backgroundColor: '#6366f1',
        paddingHorizontal: 24, paddingVertical: 12, borderRadius: 12
      }}>
        <Text style={{ color: 'white', fontWeight: '600', fontSize: 16 }}>
          Add task →
        </Text>
      </Pressable>
    </View>
  )
}
```

---

## Bottom Tab Bar (мінімалістичний)

```tsx
// app/(tabs)/_layout.tsx
import { Tabs } from 'expo-router'
import { Ionicons } from '@expo/vector-icons'

export default function TabLayout() {
  return (
    <Tabs screenOptions={{
      tabBarActiveTintColor: '#6366f1',
      tabBarInactiveTintColor: '#9ca3af',
      tabBarStyle: {
        borderTopWidth: 1,
        borderTopColor: '#f3f4f6',
        backgroundColor: 'white',
        paddingBottom: 8,
        height: 60,
      },
      tabBarLabelStyle: { fontSize: 11, fontWeight: '600' },
      headerShown: false,
    }}>
      <Tabs.Screen name="index" options={{
        title: 'Today',
        tabBarIcon: ({ color, size }) => (
          <Ionicons name="today-outline" color={color} size={size} />
        ),
      }} />
      <Tabs.Screen name="habits" options={{
        title: 'Habits',
        tabBarIcon: ({ color, size }) => (
          <Ionicons name="flame-outline" color={color} size={size} />
        ),
      }} />
      <Tabs.Screen name="stats" options={{
        title: 'Stats',
        tabBarIcon: ({ color, size }) => (
          <Ionicons name="bar-chart-outline" color={color} size={size} />
        ),
      }} />
    </Tabs>
  )
}
```

---

## FAB — Floating Action Button

```tsx
// Завжди bottom-right, 56x56, primary color
export function FAB({ onPress, icon = '+' }: { onPress: () => void; icon?: string }) {
  return (
    <Pressable
      onPress={onPress}
      style={({ pressed }) => ({
        position: 'absolute', bottom: 32, right: 24,
        width: 56, height: 56, borderRadius: 28,
        backgroundColor: '#6366f1',
        alignItems: 'center', justifyContent: 'center',
        shadowColor: '#6366f1', shadowOffset: { width: 0, height: 4 },
        shadowOpacity: 0.4, shadowRadius: 8, elevation: 8,
        opacity: pressed ? 0.85 : 1,
      })}
    >
      <Text style={{ color: 'white', fontSize: 28, lineHeight: 32 }}>{icon}</Text>
    </Pressable>
  )
}
```

---

## AI Slop Checklist (перевіряй перед демо)

**Провали:**
- [ ] Кожен item має card + shadow → Remove shadows, use dividers `borderBottomWidth: 1, borderBottomColor: '#f3f4f6'`
- [ ] Gradient background → Plain `#ffffff` або `#f9fafb`
- [ ] Всі елементи по центру → Left-align content, center only page titles
- [ ] Випадковий padding (13pt, 27pt) → Тільки multiples of 4: 8, 12, 16, 24
- [ ] 5+ розмірів шрифту → Max 3: title (22), body (17), caption (13)
- [ ] Кнопка "Submit" → "Save Task", "Mark Done", "Add Habit"
- [ ] Placeholder "Task title..." → Очисти перед демо
- [ ] ALL CAPS скрізь → Title case. ALL CAPS тільки для labels/tags

**Перевіряй:**
- [ ] Чи видно головний CTA без читання тексту?
- [ ] Чи spacing виглядає рівномірним (eye test)?
- [ ] Чи є empty state (не порожній екран)?
- [ ] Чи є loading state для async дій?
- [ ] Contrast body text ≥ 4.5:1?
