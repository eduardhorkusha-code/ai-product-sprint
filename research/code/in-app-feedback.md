# In-App Feedback — Toast / Banner (закриває темну пляму #4)
**Дата:** 2026-06-04
**Навіщо:** "fake the hard parts" — замість реальних push-notifications показуємо
in-app banner. Виглядає так само на демо, нуль налаштування OS-рівня.

---

## Toast / Banner (Reanimated, без бібліотек)

```tsx
// components/Toast.tsx
import { useEffect } from 'react'
import { Text, View } from 'react-native'
import Animated, { useSharedValue, useAnimatedStyle, withTiming, withSequence, withDelay } from 'react-native-reanimated'
import * as Haptics from 'expo-haptics'

export function Toast({ message, emoji = '🔔', visible, onHide }: {
  message: string; emoji?: string; visible: boolean; onHide: () => void
}) {
  const translateY = useSharedValue(-100)
  const opacity = useSharedValue(0)

  useEffect(() => {
    if (!visible) return
    Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)
    translateY.value = withSequence(
      withTiming(0, { duration: 300 }),
      withDelay(2500, withTiming(-100, { duration: 300 }))
    )
    opacity.value = withSequence(
      withTiming(1, { duration: 300 }),
      withDelay(2500, withTiming(0, { duration: 300 }, () => runOnJS(onHide)()))
    )
  }, [visible])

  const style = useAnimatedStyle(() => ({
    transform: [{ translateY: translateY.value }],
    opacity: opacity.value,
  }))

  if (!visible) return null
  return (
    <Animated.View style={[style, {
      position: 'absolute', top: 60, left: 16, right: 16, zIndex: 100,
      flexDirection: 'row', alignItems: 'center', gap: 12,
      backgroundColor: 'white', borderRadius: 14, padding: 16,
      shadowColor: '#000', shadowOffset: { width: 0, height: 4 },
      shadowOpacity: 0.12, shadowRadius: 12, elevation: 8,
    }]}>
      <Text style={{ fontSize: 22 }}>{emoji}</Text>
      <Text style={{ flex: 1, fontSize: 15, color: '#111827', fontWeight: '600' }}>
        {message}
      </Text>
    </Animated.View>
  )
}
```

```tsx
// Використання — "фейкове нагадування" на демо
const [toast, setToast] = useState(false)

// при якійсь дії або по таймеру для демо:
setToast(true)

<Toast
  message="Час відмітити ранкову звичку 💪"
  emoji="☀️"
  visible={toast}
  onHide={() => setToast(false)}
/>
```

> Не забудь `import { runOnJS } from 'react-native-reanimated'`.

---

## Демо-трюк: "нагадування" по таймеру
Щоб показати "push" на демо без реального push:
```tsx
// Через 3 секунди після відкриття — banner "нагадування"
useEffect(() => {
  const t = setTimeout(() => setToast(true), 3000)
  return () => clearTimeout(t)
}, [])
```
Аудиторія бачить "нагадування прилетіло" — насправді це in-app timer. Demo > Perfect.
