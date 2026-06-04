# Gemini: Швидка розробка Expo MVP з ШІ
**Джерело:** PDF "Стратегічне проектування мобільних застосунків" від Gemini Deep Research  
**Дата:** 2026-06-04  
**Нове відносно попередніх файлів:** SecureStore auth, Callstack agent-skills, 'use dom', детальна карта помилок

---

## 1. Мікро-SaaS ідеї — порівняльна таблиця

| Продукт | Wow-ефект | Аудиторія | Стек | Час збірки |
|---------|-----------|-----------|------|-----------|
| **Specsheet Mobile** | Сканує B2B PDF специфікації камерою, OCR → порівняльна таблиця | Менеджери закупівель, інженери | Expo Camera + LangChain + SQLite | ~48 год |
| **PermitSync Companion** | Геозонування будмайданчиків, push при наближенні до об'єкта | Будівельники, інспектори | Expo Location + Geofencing + OneSignal | ~48 год |
| **Podscriptor Companion** | Аудіо RSS подкасту → AI транскрипт → інтерактивний таймлайн | Творці контенту, маркетологи | Expo Audio + 'use dom' Canvas | ~60 год |
| **Offline Field Inspector** | Офлайн збір даних нерухомості + AI аналіз фото пошкоджень | Агенти, страхові комісари | WatermelonDB + Supabase Storage + локальна нейромережа | ~40 год |
| **Card Saver Assistant** | Автоматичні персоналізовані аудіо-повідомлення для покинутих кошиків | Shopify-магазини, дропшипери | Expo AV + Stripe SDK | ~36 год |

**Висновок Gemini:** найшвидші у реалізації — продукти з **локальною обробкою даних** (Specsheet, Offline Field Inspector), бо не потребують складних серверних архітектур.

---

## 2. Скільки годин економлять готові шаблони

| Компонент | Економія |
|-----------|----------|
| Stripe / RevenueCat інтеграція | **25+ год** |
| Базова auth (БД + екрани) | **15+ год** |
| EAS build + release config | **15+ год** |
| i18n локалізація | **10+ год** |
| Analytics + event tracking | **10+ год** |
| Crash-reporting (Sentry) | **8+ год** |
| Push notifications + планувальник | **8+ год** |
| Глобальна тема + Dark Mode | **6+ год** |

→ **Стартовий шаблон з усім цим = 90+ годин зекономлено**

---

## 3. Supabase — БЕЗПЕЧНИЙ клієнт (⚠️ ОНОВЛЕННЯ до нашого коду)

**Зберігання JWT у AsyncStorage = критична вразливість.**  
Треба `expo-secure-store` (iOS Keychain + Android Keystore).

```bash
npx expo install expo-secure-store react-native-url-polyfill
```

```tsx
// utils/supabase.ts  ← ПРАВИЛЬНИЙ варіант
import 'react-native-url-polyfill/auto'
import * as SecureStore from 'expo-secure-store'
import { createClient } from '@supabase/supabase-js'

const ExpoSecureStoreAdapter = {
  getItem: (key: string): Promise<string | null> =>
    SecureStore.getItemAsync(key),
  setItem: (key: string, value: string): Promise<void> =>
    SecureStore.setItemAsync(key, value),
  removeItem: (key: string): Promise<void> =>
    SecureStore.deleteItemAsync(key),
}

export const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL as string,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY as string,
  {
    auth: {
      storage: ExpoSecureStoreAdapter as any,
      autoRefreshToken: true,
      persistSession: true,
      detectSessionInUrl: false, // ← запобігає конфліктам deeplinks в SDK
    },
  }
)
```

---

## 4. Auth бар'єри в Expo Router (декларативний підхід)

### app/_layout.tsx — root (керує splash screen)
```tsx
// app/_layout.tsx
import { useEffect, useState } from 'react'
import { Slot, useRouter } from 'expo-router'
import { supabase } from '@/utils/supabase'

SplashScreen.preventAutoHideAsync()

export default function RootLayout() {
  const [loading, setLoading] = useState(true)
  const router = useRouter()

  useEffect(() => {
    // Одноразова перевірка сесії при старті
    supabase.auth.getSession().then(({ data: { session } }) => {
      setLoading(false)
      SplashScreen.hideAsync()
      if (session) router.replace('/(tabs)/home')
      else router.replace('/(auth)/login')
    })

    // Підписка на динамічні зміни стану авторизації
    const { data } = supabase.auth.onAuthStateChange((_event, session) => {
      if (session) router.replace('/(tabs)/home')
      else router.replace('/(auth)/login')
    })
    return () => data.subscription.unsubscribe()
  }, [])

  if (loading) return null
  return <Slot />
}
```

### app/(tabs)/_layout.tsx — захищений макет
```tsx
// app/(tabs)/_layout.tsx
import { Redirect } from 'expo-router'
import { useState, useEffect } from 'react'
import { supabase } from '@/utils/supabase'

export default function TabsLayout() {
  const [session, setSession] = useState<boolean | null>(null)

  useEffect(() => {
    supabase.auth.getSession(({ data: { session } }) => {
      setSession(!!session)
    })
  }, [])

  if (session === null) return null
  if (!session) return <Redirect href="/(auth)/login" />

  return (
    <Tabs screenOptions={{ headerShown: false }}>
      <Tabs.Screen name="home" options={{ title: 'Панель' }} />
      <Tabs.Screen name="profile" options={{ title: 'Профіль' }} />
    </Tabs>
  )
}
```

---

## 5. 'use dom' — веб-компоненти в нативному апп

**Навіщо:** рендерити D3.js, Monaco editor, Rich Text — без нативного аналогу.  
**Як:** Expo ізолює у системний WebView з повним доступом до DOM.

```tsx
// components/chart-webview.tsx  ← виконується в браузері
'use dom'
import { useState } from 'react'

interface ChartProps {
  data: Array<{ label: string; value: number }>
  onSelectNode: (label: string) => void
}

export default function InteractiveChart({ data, onSelectNode }: ChartProps) {
  const mountRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (!mountRef.current) return
    const container = document.createElement('div')
    container.style.display = 'flex'
    container.style.flexDirection = 'column'
    container.style.gap = '8px'

    data.forEach((item) => {
      const wrapper = document.createElement('div')
      wrapper.style.cursor = 'pointer'
      const bar = document.createElement('div')
      bar.style.background = '#fff'
      bar.style.borderRadius = '12px'
      bar.style.padding = '12px'
      const fill = document.createElement('div')
      fill.style.height = '16px'
      fill.innerHTML = `${item.value}%`
      fill.style.background = '#bd93f9'
      fill.style.borderRadius = '4px'
      wrapper.onclick = () => onSelectNode(item.label)
      wrapper.appendChild(bar)
      bar.appendChild(fill)
      container.appendChild(wrapper)
    })
    mountRef.current.appendChild(container)
  }, [data])

  return <div ref={mountRef} style={{ width: '100%', height: '100%', padding: '12px', backgroundColor: '#1e1e24' }} />
}
```

```tsx
// app/(tabs)/home.tsx — нативний батьківський екран
import { View, Text } from 'react-native'
import { useRouter } from 'expo-router'
import * as Haptics from 'expo-haptics'
import InteractiveChart from '@/components/chart-webview'

const metrics = [
  { label: 'Завантаження процесора', value: 72 },
  { label: 'Використання пам\'яті', value: 48 },
  { label: 'Дискова активність', value: 91 },
]

export default function HomeScreen() {
  const router = useRouter()

  const handleNodeSelection = async (label: string) => {
    // Тактильний відгук на рівні ОС при кліку у WebView
    await Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium)
    router.push(`/details?metric=${encodeURIComponent(label)}`)
  }

  return (
    <View style={{ flex: 1, backgroundColor: '#121214', padding: 20, paddingTop: 50 }}>
      <Text style={{ fontSize: 22, fontWeight: 'bold', color: '#fff', marginBottom: 15 }}>
        Системна телеметрія
      </Text>
      <View style={{ width: '100%', height: 250, borderRadius: 8, overflow: 'hidden' }}>
        <InteractiveChart
          data={metrics}
          onSelectNode={handleNodeSelection}
          dom={{ scrollEnabled: false }} // нативні налаштування WebView
        />
      </View>
    </View>
  )
}
```

---

## 6. Погодинний план (версія Gemini — деталізований)

| Час | Фаза | Маркер готовності |
|-----|------|------------------|
| 08:00–09:00 | Ініціалізація архітектури | app/, components/ui/, utils/ створено; NativeWind підключено |
| 09:00–10:30 | Модель даних + Supabase | Таблиці створені, RLS активовано, SecureStore клієнт тестується |
| 10:30–12:00 | Auth + маршрутизація | Екрани login/signup, авторизаційні бар'єри, переходи при зміні сесії |
| 12:00–14:00 | Ключова бізнес-логіка | CRUD, обробка даних, Error Boundary на рівні екранів |
| 14:00–15:30 | Візуалізація + інтеграції | Haptics, 'use dom' компоненти або складні інструменти |
| 15:30–16:30 | Crash-reporting + телеметрія | Sentry SDK, глобальні перехоплювачі винятків, аналітика |
| 16:30–17:30 | TypeScript + аналіз бандлу | `bun run typecheck` = 0 помилок, Expo Atlas — розмір OK |
| 17:30–18:00 | EAS хмарна компіляція | Жодних секретів у Git, відддалений білд запущено |

---

## 7. Протокол деескалації — що РІЗАТИ

| Що різати | Чому | Економія |
|-----------|------|----------|
| Кастомні UI елементи з нуля | Використай gluestack UI / NativeWind UI / Tamagui | **10+ год** |
| Багатомовна локалізація (i18n) | Hardcode першу мову | **10+ год** |
| Складні нативні анімації | `react-native-screens` + базова інтерполяція Reanimated | 3–5 год |
| Deep Linking вкладена навігація | Базовий автоматичний маппінг шляхів | 2–3 год |

---

## 8. Callstack Agent-Skills — ЗОЛОТО для нас ⭐

**Репо:** https://github.com/callstackincubator/agent-skills  
**Що це:** колекція skills для AI coding assistants оптимізованих під React Native.

Плагін автоматично передає агенту:
- Задокументовані патерни оптимізації FPS
- Виявлення витоків пам'яті
- Правильне налаштування компіляторів для мобільних платформ

```bash
# Встановлення в проект
npx @callstack/react-native-ai-skills init
npx @callstack/react-native-ai-skills --add-to-claude-code
```

→ **Зробити до буткемпу: клонувати, прочитати skills, додати в CLAUDE.md**

---

## 9. Карта критичних помилок (нові — яких немає в нашому файлі)

### 🔴 NativeWind v4 Fast Refresh зависає > 30 сек
**Симптом:** при збереженні файлу з новими Tailwind класами Metro зависає.  
**Причина:** занадто широкі маски в `tailwind.config.js` (наприклад, `/*.ts`) → сканує `node_modules`.  
**Фікс:**
```js
// tailwind.config.js — ТІЛЬКИ ці шляхи!
content: [
  './app/**/*.{js,jsx,ts,tsx}',
  './components/**/*.{js,jsx,ts,tsx}',
  // НЕ додавай './screens/**' або '/*.ts'
]
```

### 🔴 DEVELOPER_ERROR у Google Auth + Supabase
**Симптом:** `DEVELOPER_ERROR` без деталей при Google Sign-In.  
**Причина:** передано Android Client ID замість Web Client ID у `webClientId`.  
**Фікс:** у Google Cloud Console → створи 3 типи клієнтів (iOS, Android, Web) → у Supabase і в коді використовуй саме **Web Client ID**.

### 🔴 EAS Build «None of these files exist»
**Симптом:** локальна збірка OK, EAS хмарна — падає на імпорті.  
**Причина:** `.env` файл в `.gitignore`, EAS завантажує тільки tracked файли → прямий імпорт ламається.  
**Фікс:** чутливі дані → EAS Secrets (особистий кабінет) → відновлювати через EAS Build Hooks.  
```bash
eas secret:create --scope project --name SUPABASE_URL --value "https://..."
```

### 🔴 WebView зависає в Android Expo Go з 'use dom'
**Симптом:** на iOS працює, на Android — порожній екран через кілька мс.  
**Причина:** баг SystemWebView в Expo Go на Android при відсутності явних розмірів.  
**Фікс:** завжди задавай явні розміри батьківського контейнера:
```tsx
<View style={{ width: '100%', height: 250 }}>  {/* НЕ flex: 1 */}
  <MyDomComponent dom={{ scrollEnabled: false }} />
</View>
```
**Примітка:** для стабільного тестування 'use dom' на Android — тільки Development Build, не Expo Go.

### 🟡 Metro «TypeError: Invalid URL» при Hot Reload
**Симптом:** аварійне завершення при гарячому оновленні з помилкою валідації URL.  
**Причина:** жорсткі `resolutions` у `package.json` перевизначають версію Metro.  
**Фікс:** видалити будь-які Metro `resolutions/overrides` з `package.json`.

### 🟡 CocoaPods не знайдено через rbenv (macOS)
**Симптом:** `pod install` не запускається у Expo CLI.  
**Причина:** rbenv не ініціалізується в неінтерактивних сесіях терміналу.  
**Фікс:** додати у `~/.zshrc`:
```bash
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```
Потім: `bundle exec pod deintegrate && bundle exec pod install`.

### 🟡 EMFILE: too many open files
**Симптом:** `Error: EMFILE: too many open files` при індексації Metro.  
**Причина:** Watchman перевищує ліміти macOS для монорепозиторіїв.  
**Фікс:**
```bash
watchman watch-del-all
watchman shutdown-server
```

---

## 10. Claude Code налаштування — нові деталі

```bash
# Env variable що усуває мигання UI в терміналі
export CLAUDE_CODE_NO_FLICKER=1

# Додай у ~/.zshrc або .env
```

**Ієрархія конфігів:**
```
~/.claude/settings.json          ← глобальні (всі проекти)
.claude/settings.json            ← проектні (в Git, для команди)
.claude/settings.local.json      ← локальні (в .gitignore, для тестів)
```

**Команди для довгих сесій:**
```
/compact   ← стискає діалог зі збереженням архітектурних рішень
/clear     ← повне очищення при переході між задачами
```

### CLAUDE.md для Expo + NativeWind (версія Gemini)

```markdown
# Специфікація проекту Expo & NativeWind

## Системні команди розробки
- Встановлення модулів: bun install (ЗАБОРОНЕНО npm або yarn)
- Локальний сервер: bunx expo start
- Аналіз типізації: bun run typecheck (tsc --noEmit)
- Локальний експорт бандла: bunx expo export
- Збірка через EAS: eas build --platform android

## Архітектурні обмеження
- Маршрутизація: тільки типізований Expo Router v3+ (typedRoutes: true)
- Безпека: зберігання токенів ВИКЛЮЧНО через SecureStore Adapter у Supabase
- Стилізація: класи NativeWind v4, ЗАБОРОНЕНО inline-стилі StyleSheet
- Асинхронні мости: всі веб-компоненти з 'use dom' отримують ВИКЛЮЧНО серіалізовані пропси

## Верифікація перед commit
Завжди запускати bun run typecheck перед завершенням завдання.
```

---

## 11. Ключові джерела з PDF (варто прочитати)

| Джерело | Що там |
|---------|--------|
| [callstackincubator/agent-skills](https://github.com/callstackincubator/agent-skills) | React Native skills для AI агентів — MUST READ |
| [nativelaunch.dev/expo-supabase-template](https://nativelaunch.dev/expo-supabase-template) | Expo + Supabase production template |
| [expo.dev/blog/how-hipcamp-upgraded-expo-sdk-versions-with-claude-code](https://expo.dev/blog/how-hipcamp-upgraded-expo-sdk-versions-with-claude-code) | Реальний кейс Expo + Claude Code |
| [medium.com/cars24/claude-code-for-react-react-native-workflows](https://medium.com/cars24/claude-code-for-react-react-native-workflows-that-actually-move-the-needle-33b8bb410b14) | Claude Code workflows що дають результат |
| [reddit/vibecoding "Claude wrote 80% of my RN app"](https://www.reddit.com/r/vibecoding/comments/1s6dqam/claude_wrote_80_of_my_react_native_app_then_i_hit/) | Де AI зупиняється — реальний досвід |
| [dev.to NativeWind 2026 setup guide](https://dev.to/nabinkdl/complete-setup-guide-expo-nativewind-2026-edition-26i1) | Повний setup Expo + NativeWind 2026 |
| [expo.dev/blog/dom-component-use-case](https://expo.dev/blog/dom-component-use-case) | 'use dom' офіційний гайд |
