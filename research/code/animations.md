# Animations — Copy-Paste Ready
**Оновлено:** 2026-06-04  
**Правило:** кожна анімація — max 30 хвилин, вражає нетехнічну аудиторію

---

## Setup (один раз)

```bash
npx expo install react-native-reanimated react-native-gesture-handler
```

```js
// babel.config.js — додай плагін
plugins: ['react-native-reanimated/plugin']
```

```tsx
// app/_layout.tsx
import 'react-native-gesture-handler'
import { GestureHandlerRootView } from 'react-native-gesture-handler'

export default function Layout() {
  return (
    <GestureHandlerRootView style={{ flex: 1 }}>
      <Stack />
    </GestureHandlerRootView>
  )
}
```

---

## 1. Checkbox Bounce — 5 хвилин ⭐⭐⭐
**Ефект:** тап → пружна компресія → відновлення + haptic  
**Де:** task item, habit check, будь-який toggle

```tsx
import { Pressable } from 'react-native'
import Animated, { useSharedValue, useAnimatedStyle, withSpring } from 'react-native-reanimated'
import * as Haptics from 'expo-haptics'

const AnimatedPressable = Animated.createAnimatedComponent(Pressable)

export function CheckButton({ done, onToggle }: { done: boolean; onToggle: () => void }) {
  const scale = useSharedValue(1)
  
  const style = useAnimatedStyle(() => ({
    transform: [{ scale: scale.value }]
  }))
  
  const handlePress = () => {
    scale.value = withSpring(0.85, { damping: 4, stiffness: 200 }, () => {
      scale.value = withSpring(1, { damping: 6, stiffness: 300 })
    })
    Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)
    onToggle()
  }
  
  return (
    <AnimatedPressable onPress={handlePress} style={[style, {
      width: 28, height: 28, borderRadius: 14,
      borderWidth: 2, borderColor: '#6366f1',
      backgroundColor: done ? '#6366f1' : 'transparent',
      alignItems: 'center', justifyContent: 'center',
    }]}>
      {done && <Text style={{ color: 'white', fontSize: 14 }}>✓</Text>}
    </AnimatedPressable>
  )
}
```

---

## 2. Slide-in List Items — 10 хвилин ⭐⭐⭐
**Ефект:** кожен item влітає зліва з затримкою (staggered)  
**Де:** при першому завантаженні списку → враження polish

```tsx
import Animated, { 
  useSharedValue, useAnimatedStyle, withTiming, withDelay, FadeInLeft
} from 'react-native-reanimated'

// Варіант 1 — через entering prop (простіший)
export function AnimatedItem({ item, index }: { item: Task; index: number }) {
  return (
    <Animated.View entering={FadeInLeft.delay(index * 80).duration(300)}>
      <TaskRow item={item} />
    </Animated.View>
  )
}

// Варіант 2 — ручний (більше контролю)
export function AnimatedItemManual({ item, index }: { item: Task; index: number }) {
  const opacity = useSharedValue(0)
  const translateX = useSharedValue(-30)
  
  useEffect(() => {
    const delay = index * 80
    opacity.value = withDelay(delay, withTiming(1, { duration: 300 }))
    translateX.value = withDelay(delay, withTiming(0, { duration: 300 }))
  }, [])
  
  const style = useAnimatedStyle(() => ({
    opacity: opacity.value,
    transform: [{ translateX: translateX.value }]
  }))
  
  return <Animated.View style={style}><TaskRow item={item} /></Animated.View>
}
```

---

## 3. Swipe-to-Delete — 20 хвилин ⭐⭐⭐⭐
**Ефект:** свайп вліво → червона зона → відпустив → видалення з колапсом  
**Де:** будь-який список з видаленням

```tsx
import { Gesture, GestureDetector } from 'react-native-gesture-handler'
import Animated, {
  useSharedValue, useAnimatedStyle, withSpring, withTiming, runOnJS
} from 'react-native-reanimated'
import { Dimensions } from 'react-native'

const SCREEN_WIDTH = Dimensions.get('window').width
const SWIPE_THRESHOLD = -SCREEN_WIDTH * 0.35

export function SwipeableRow({ children, onDelete }: { children: React.ReactNode; onDelete: () => void }) {
  const translateX = useSharedValue(0)
  const rowHeight = useSharedValue(72)
  const opacity = useSharedValue(1)
  
  const gesture = Gesture.Pan()
    .activeOffsetX([-10, 10])
    .onUpdate((e) => {
      if (e.translationX < 0) translateX.value = e.translationX
    })
    .onEnd((e) => {
      if (e.translationX < SWIPE_THRESHOLD) {
        translateX.value = withTiming(-SCREEN_WIDTH)
        rowHeight.value = withTiming(0, { duration: 250 })
        opacity.value = withTiming(0, { duration: 250 }, () => runOnJS(onDelete)())
      } else {
        translateX.value = withSpring(0)
      }
    })
  
  const rowStyle = useAnimatedStyle(() => ({
    transform: [{ translateX: translateX.value }],
    height: rowHeight.value,
    opacity: opacity.value,
    overflow: 'hidden',
  }))
  
  const deleteStyle = useAnimatedStyle(() => ({
    opacity: translateX.value < -20 ? 1 : 0,
  }))
  
  return (
    <View style={{ position: 'relative' }}>
      {/* Delete background */}
      <Animated.View style={[deleteStyle, {
        position: 'absolute', right: 0, top: 0, bottom: 0,
        width: 80, backgroundColor: '#ef4444',
        alignItems: 'center', justifyContent: 'center',
      }]}>
        <Text style={{ color: 'white', fontSize: 20 }}>🗑</Text>
      </Animated.View>
      
      {/* Row */}
      <GestureDetector gesture={gesture}>
        <Animated.View style={rowStyle}>
          {children}
        </Animated.View>
      </GestureDetector>
    </View>
  )
}
```

---

## 4. Progress Ring — 15 хвилин ⭐⭐⭐⭐
**Ефект:** SVG circle заповнюється анімовано  
**Де:** streak counter, habit completion rate, daily progress

```tsx
import Animated, { useSharedValue, useAnimatedProps, withTiming } from 'react-native-reanimated'
import Svg, { Circle } from 'react-native-svg'

const AnimatedCircle = Animated.createAnimatedComponent(Circle)

export function ProgressRing({ progress, size = 80, color = '#6366f1' }: {
  progress: number // 0-1
  size?: number
  color?: string
}) {
  const radius = (size - 8) / 2
  const circumference = 2 * Math.PI * radius
  const animatedProgress = useSharedValue(0)
  
  useEffect(() => {
    animatedProgress.value = withTiming(progress, { duration: 800 })
  }, [progress])
  
  const animatedProps = useAnimatedProps(() => ({
    strokeDashoffset: circumference * (1 - animatedProgress.value)
  }))
  
  return (
    <Svg width={size} height={size} style={{ transform: [{ rotate: '-90deg' }] }}>
      <Circle cx={size/2} cy={size/2} r={radius}
        stroke="#e5e7eb" strokeWidth={6} fill="none" />
      <AnimatedCircle
        cx={size/2} cy={size/2} r={radius}
        stroke={color} strokeWidth={6} fill="none"
        strokeDasharray={circumference}
        strokeLinecap="round"
        animatedProps={animatedProps}
      />
    </Svg>
  )
}
```

```bash
npx expo install react-native-svg
```

---

## 5. Streak Fire Counter — 10 хвилин ⭐⭐⭐⭐⭐
**Ефект:** число збільшується + fire emoji пульсує + haptic  
**Де:** головний екран habit tracker / win journal — це і є wow-момент

```tsx
import Animated, {
  useSharedValue, useAnimatedStyle, withSequence, withSpring, withTiming
} from 'react-native-reanimated'
import * as Haptics from 'expo-haptics'

export function StreakCounter({ count }: { count: number }) {
  const fireScale = useSharedValue(1)
  const numberScale = useSharedValue(1)
  
  useEffect(() => {
    // Fire pulse
    fireScale.value = withSequence(
      withSpring(1.4, { damping: 4 }),
      withSpring(1, { damping: 6 })
    )
    // Number pop
    numberScale.value = withSequence(
      withTiming(1.3, { duration: 150 }),
      withSpring(1, { damping: 5 })
    )
    if (count > 0) Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)
  }, [count])
  
  const fireStyle = useAnimatedStyle(() => ({
    transform: [{ scale: fireScale.value }]
  }))
  const numberStyle = useAnimatedStyle(() => ({
    transform: [{ scale: numberScale.value }]
  }))
  
  return (
    <View style={{ alignItems: 'center', flexDirection: 'row', gap: 8 }}>
      <Animated.Text style={[fireStyle, { fontSize: 32 }]}>🔥</Animated.Text>
      <Animated.Text style={[numberStyle, {
        fontSize: 48, fontWeight: '800', color: '#f97316'
      }]}>
        {count}
      </Animated.Text>
      <Text style={{ fontSize: 16, color: '#9ca3af', alignSelf: 'flex-end', marginBottom: 8 }}>
        days
      </Text>
    </View>
  )
}
```

---

## 6. Bottom Sheet — 25 хвилин ⭐⭐⭐
**Де:** Add task / Add habit / Details без навігації

```tsx
import Animated, {
  useSharedValue, useAnimatedStyle, withSpring, runOnJS
} from 'react-native-reanimated'
import { Gesture, GestureDetector } from 'react-native-gesture-handler'
import { Dimensions, Modal, Pressable } from 'react-native'

const SCREEN_HEIGHT = Dimensions.get('window').height
const SHEET_HEIGHT = SCREEN_HEIGHT * 0.6

export function BottomSheet({ visible, onClose, children }: {
  visible: boolean; onClose: () => void; children: React.ReactNode
}) {
  const translateY = useSharedValue(SHEET_HEIGHT)
  
  useEffect(() => {
    translateY.value = withSpring(visible ? 0 : SHEET_HEIGHT, {
      damping: 20, stiffness: 200
    })
  }, [visible])
  
  const gesture = Gesture.Pan()
    .onUpdate((e) => {
      if (e.translationY > 0) translateY.value = e.translationY
    })
    .onEnd((e) => {
      if (e.translationY > SHEET_HEIGHT * 0.3 || e.velocityY > 500) {
        translateY.value = withSpring(SHEET_HEIGHT)
        runOnJS(onClose)()
      } else {
        translateY.value = withSpring(0)
      }
    })
  
  const sheetStyle = useAnimatedStyle(() => ({
    transform: [{ translateY: translateY.value }]
  }))
  
  return (
    <Modal transparent visible={visible} onRequestClose={onClose}>
      <Pressable style={{ flex: 1, backgroundColor: 'rgba(0,0,0,0.4)' }} onPress={onClose} />
      <GestureDetector gesture={gesture}>
        <Animated.View style={[sheetStyle, {
          position: 'absolute', bottom: 0, left: 0, right: 0,
          height: SHEET_HEIGHT, backgroundColor: 'white',
          borderTopLeftRadius: 20, borderTopRightRadius: 20,
          padding: 20,
        }]}>
          {/* Drag handle */}
          <View style={{ width: 36, height: 4, backgroundColor: '#d1d5db',
            borderRadius: 2, alignSelf: 'center', marginBottom: 16 }} />
          {children}
        </Animated.View>
      </GestureDetector>
    </Modal>
  )
}
```

---

## Бібліотека альтернатив (якщо немає часу писати самому)

| Потреба | Бібліотека | Install |
|---------|-----------|---------|
| Bottom Sheet | `@gorhom/bottom-sheet` | `npx expo install @gorhom/bottom-sheet` |
| Swipeable rows | `react-native-swipeable-item` | `npm install react-native-swipeable-item` |
| SVG charts | `victory-native` | `npx expo install victory-native` |
| Lottie animations | `lottie-react-native` | `npx expo install lottie-react-native` |
| Confetti | `react-native-confetti-cannon` | `npm install react-native-confetti-cannon` |
