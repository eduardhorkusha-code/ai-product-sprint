# Starter Templates — Ready to Clone
**Оновлено:** 2026-06-04  
**Мета:** не починати з нуля, клонувати найближчий шаблон і одразу будувати фічу

---

## Топ шаблон для буткемпу

### ⭐ `toyamarodrigo/expo-router-template`
**Посилання:** https://github.com/toyamarodrigo/expo-router-template  
**Стек:** Expo 54 + Expo Router 6 + TypeScript + Zustand + TanStack Query + NativeWind 4 + Reanimated + Zod + FlashList + Bun  
**Чому топ:** все що треба вже налаштовано, один `bun install` і можна будувати

```bash
bunx create-expo-app --template https://github.com/toyamarodrigo/expo-router-template
```

---

### `chvvkrishnakumar/expo-nativewind-template`
**Посилання:** https://github.com/chvvkrishnakumar/expo-nativewind-template  
**Стек:** Expo + NativeWind + shadcn-style components + dark mode + Expo Router  
**Включає:** 20+ готових компонентів — Buttons, Cards, Dialogs, Bottom Sheets  
**Коли брати:** хочеш готову дизайн-систему з коробки

---

### `Sonnysam/starter-template-expo`
**Посилання:** https://github.com/Sonnysam/starter-template-expo  
**Стек:** Expo + NativeWind + Supabase + Firebase + Zustand + TypeScript + Auth flow  
**Включає:** 3 bottom tabs, auth screens, home screen  
**Коли брати:** потрібен auth + Supabase з першого рядка

---

### `artsiomshaitar/habits-tracker-react-native`
**Посилання:** https://github.com/artsiomshaitar/habits-tracker-react-native  
**Стек:** React Native + Expo + Tailwind  
**Що показує:** daily/weekly habits, progress, local storage  
**Коли брати:** якщо будуємо habit tracker — дивитись на архітектуру

---

### `hasibhaque07/open-source-habit-tracker-app`
**Посилання:** https://github.com/hasibhaque07/open-source-habit-tracker-app  
**Стек:** React Native + Expo + SQLite (local)  
**Що показує:** мінімальний habit tracker без backend  
**Коли брати:** референс для simple local-first архітектури

---

## Швидкий старт — команди

```bash
# Варіант 1: з найкращого шаблону (рекомендовано)
bunx create-expo-app MyApp --template https://github.com/toyamarodrigo/expo-router-template

# Варіант 2: чистий Expo з NativeWind
npx create-expo-app MyApp --template blank-typescript
cd MyApp && npx expo install nativewind expo-router react-native-reanimated

# Варіант 3: офіційний Expo з tabs
npx create-expo-app MyApp --template tabs
```

---

## Структура файлів (sprint-ready)

```
app/
  (tabs)/
    index.tsx          ← головний екран (aha-момент тут)
    _layout.tsx        ← tabs config
  _layout.tsx          ← root layout, fonts, providers
  +not-found.tsx
components/
  [Feature].tsx        ← 1 компонент = 1 файл, без split
lib/
  storage.ts           ← AsyncStorage helpers
  types.ts             ← TypeScript interfaces
  supabase.ts          ← тільки якщо потрібен backend
assets/
  fonts/               ← Inter або Poppins
```

---

## Шрифти (копіюй одразу)

```tsx
// app/_layout.tsx
import { useFonts, Inter_400Regular, Inter_600SemiBold, Inter_700Bold } from '@expo-google-fonts/inter'
import * as SplashScreen from 'expo-splash-screen'

SplashScreen.preventAutoHideAsync()

export default function RootLayout() {
  const [fontsLoaded] = useFonts({ Inter_400Regular, Inter_600SemiBold, Inter_700Bold })
  
  useEffect(() => {
    if (fontsLoaded) SplashScreen.hideAsync()
  }, [fontsLoaded])

  if (!fontsLoaded) return null
  return <Stack />
}
```

```bash
npx expo install @expo-google-fonts/inter expo-font expo-splash-screen
```

---

## NativeWind v4 setup (5 хвилин)

```bash
npx expo install nativewind tailwindcss react-native-reanimated react-native-safe-area-context
npx tailwindcss init
```

```js
// tailwind.config.js
module.exports = {
  content: ["./app/**/*.{js,jsx,ts,tsx}", "./components/**/*.{js,jsx,ts,tsx}"],
  presets: [require("nativewind/preset")],
  theme: { extend: {} },
  plugins: [],
}
```

```js
// babel.config.js
module.exports = function(api) {
  api.cache(true)
  return {
    presets: [
      ["babel-preset-expo", { jsxImportSource: "nativewind" }],
      "nativewind/babel",
    ],
  }
}
```

```tsx
// app.d.ts (для TypeScript)
/// <reference types="nativewind/types" />
```
