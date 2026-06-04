# Claude Code + Expo — Оптимальне налаштування
**Оновлено:** 2026-06-04  
**Мета:** мінімум токенів, максимум точності під час буткемпу

---

## CLAUDE.md для Expo проекту (готовий шаблон)

```markdown
# [ProjectName] — [1 sentence description]

## Stack
- Expo SDK 53 + Expo Router v3 (file-based routing)
- React Native + NativeWind v4 (Tailwind CSS)
- React Native Reanimated 3 + Gesture Handler
- Zustand (state) + AsyncStorage (persistence)
- TypeScript strict mode

## Key Files
\`\`\`
app/(tabs)/index.tsx      — main screen
app/(tabs)/_layout.tsx    — tab navigation
components/               — 1 component = 1 file
lib/types.ts              — all TypeScript interfaces
lib/storage.ts            — AsyncStorage helpers
lib/store.ts              — Zustand stores
lib/colors.ts             — color system
\`\`\`

## Rules
- Imports: use @/ path alias (configured in tsconfig)
- AsyncStorage: always use lib/storage.ts helpers, never direct calls
- IDs: always nanoid() from 'nanoid/non-secure'
- Dates: always ISO string toISOString().split('T')[0]
- No useEffect for navigation — use router.push()
- No inline styles — NativeWind classes or StyleSheet
- Spacing: multiples of 4 only (8, 12, 16, 24, 32)
- Max 3 font sizes: 22 (title), 17 (body), 13 (caption)

## Known Patterns
- Animation: see research/code/animations.md
- State: Zustand store in lib/store.ts
- Lists: FlashList (not FlatList)

## Expo-specific
- expo-haptics for all interactive feedback
- expo-sharing + react-native-view-shot for share cards
- Use Ionicons (@expo/vector-icons) for icons
- SplashScreen.preventAutoHideAsync() in root layout

## What NOT to build
- No notifications — use in-app UI
- No OAuth — local device ID or Supabase email
- No custom animations > 30 min — use library instead
```

---

## Найчастіші помилки Claude Code у Expo

| Помилка моделі | Як запобігти |
|----------------|-------------|
| Імпортує `import { useState } from 'react-native'` | В CLAUDE.md: "React hooks import from 'react', not 'react-native'" |
| Використовує застарілий Expo API (Camera, MediaLibrary) | В CLAUDE.md: "expo-camera v14+, use useCameraPermissions hook" |
| `StyleSheet.create()` замість NativeWind | В CLAUDE.md: "Styling: NativeWind classes only, no StyleSheet" |
| `navigation.navigate()` замість Expo Router | В CLAUDE.md: "Navigation: use router.push() from expo-router, no React Navigation" |
| `FlatList` замість `FlashList` | В CLAUDE.md: "Lists: always FlashList from @shopify/flash-list" |
| Supabase client на module level | В CLAUDE.md: "Supabase: create client inside function, never at module level" |
| `Math.random()` для ID | В CLAUDE.md: "IDs: always nanoid() from 'nanoid/non-secure'" |

---

## Паралельна робота агентів під час буткемпу

### Сценарій 1: Design + Dev паралельно
```
Головна сесія (ти):
  → Розставляєш задачі, приймаєш рішення

Агент A (claude --worktree design):
  → Refines UI components, spacing, colors
  → Читає research/design/ui-patterns.md
  
Агент B (головна сесія):
  → Будує core logic та state management
  → Читає research/code/core-patterns.md
```

### Сценарій 2: Feature + Animation паралельно
```
Агент A: реалізує core data flow (CRUD, storage)
Агент B: робить анімацію для вже готового компонента
         (дає йому статичний компонент як input)
```

### Команди для worktree
```bash
# Відкрити паралельну сесію для UI
claude --worktree ui-polish

# Переглянути активні сесії
claude agents

# Worktree автовидалиться після merge
```

---

## Підготовка агентів ДО буткемпу (зроби заздалегідь)

### 1. Перевір що CLAUDE.md є в репо
```bash
ls -la .claude/ && cat CLAUDE.md
```

### 2. Протестуй шаблон
```bash
npx create-expo-app TestApp --template https://github.com/toyamarodrigo/expo-router-template
cd TestApp && bun install && npx expo start
```
Переконайся що Expo Go відкриває без помилок на телефоні.

### 3. Перевір MCP (якщо потрібен Supabase)
```bash
# Supabase CLI
npm install -g supabase
supabase --version
```

### 4. Seed data заготовлена
```tsx
// lib/seed.ts — для demo mode
export const SEED_HABITS: Habit[] = [
  { id: '1', name: 'Morning water', emoji: '💧', color: '#3b82f6', completedDates: [...last14days] },
  { id: '2', name: 'Read 20 min', emoji: '📚', color: '#10b981', completedDates: [...last10days] },
  { id: '3', name: 'Evening walk', emoji: '🚶', color: '#f59e0b', completedDates: [...last7days] },
]
```

---

## Expo Go vs Dev Build — коли що

| Ситуація | Використовуй |
|----------|-------------|
| Буткемп (швидко, просто) | **Expo Go** — install з App Store, scan QR |
| NativeWind не аплається | Перезапусти Metro: `expo start --clear` |
| Потрібен кастомний нативний модуль | Dev build: `npx expo run:ios` (30+ хвилин, не для буткемпу) |
| Simulator для демо | `npx expo start --ios` |

**Порада:** на буткемпі — тільки Expo Go. Не витрачай час на dev build.
