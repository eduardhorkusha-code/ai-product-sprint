# Module: Payment Flow — paywall → підписка
**Джерело:** RevenueCat docs (Expo), web search 2025
**Дата:** 2026-06-04

---

## ⚠️ КРИТИЧНО для буткемпу
**Реальні in-app purchases НЕ працюють у Expo Go — потрібен dev build.**
У 4-годинному демо на Expo Go → **МОКАЄМО платіж.** Виглядає ідентично, нуль налаштування сторів.
RevenueCat — для прод-версії (нижче), не для демо.

---

## 🎬 ДЕМО-режим (mock, ~10 хв) — це беремо

### `lib/subscription.ts` — стан підписки
```typescript
import { create } from 'zustand'
import { persist, createJSONStorage } from 'zustand/middleware'
import AsyncStorage from '@react-native-async-storage/async-storage'

interface SubState {
  isPro: boolean
  plan: 'free' | 'monthly' | 'annual'
  subscribe: (plan: 'monthly' | 'annual') => void
  restore: () => void
  reset: () => void
}
export const useSubscription = create<SubState>()(
  persist(
    (set) => ({
      isPro: false, plan: 'free',
      subscribe: (plan) => set({ isPro: true, plan }),  // на демо — миттєво
      restore: () => set({ isPro: true, plan: 'annual' }),
      reset: () => set({ isPro: false, plan: 'free' }),
    }),
    { name: 'sub', storage: createJSONStorage(() => AsyncStorage) }
  )
)
```

### Гейтинг контенту
```tsx
const { isPro } = useSubscription()
// у будь-якому екрані:
if (!isPro) return <Paywall onSubscribe={(plan) => subscribe(plan)} />
return <PremiumContent />
```

Paywall-екран — бери з [onboarding-funnel.md](onboarding-funnel.md) §Paywall.
Кнопка "Почати 7 днів" → `subscribe('annual')` → isPro=true → контент розблоковано.
На демо: "Тут підключається App Store / RevenueCat — у проді реальна транзакція."

---

## 🏭 ПРОД-режим (RevenueCat) — для довідки, НЕ на демо

```bash
npx expo install expo-dev-client react-native-purchases react-native-purchases-ui
# потрібен dev build: npx expo run:ios
```

### Ініціалізація (entry point)
```typescript
import Purchases from 'react-native-purchases'

Purchases.configure({
  apiKey: Platform.select({
    ios: process.env.EXPO_PUBLIC_RC_IOS_KEY!,
    android: process.env.EXPO_PUBLIC_RC_ANDROID_KEY!,
  })!,
})
```

### Показати paywall (готовий UI з дашборду RevenueCat)
```tsx
import RevenueCatUI from 'react-native-purchases-ui'

const { customerInfo } = await RevenueCatUI.presentPaywall()
const isPro = customerInfo.entitlements.active['pro'] !== undefined
```

### Перевірка підписки
```typescript
const info = await Purchases.getCustomerInfo()
const isPro = info.entitlements.active['pro'] !== undefined
```

**Концепти RevenueCat:**
- **Offerings** — набори продуктів (monthly/annual), міняєш ціни без релізу апа
- **Entitlements** — права доступу ('pro'), привʼязані до продуктів
- **Paywall** — UI будується в дашборді RevenueCat, тягнеться в апп

### Web2app (як у SKELAR)
Web-воронка → оплата Stripe/Paddle на вебі → app розблоковує через RevenueCat
(спільний customer по email/id). web2app paywall ~6% (3× App Store).

---

## Що сказати в пітчі
"Payment через RevenueCat — App Store + Play + Web одним SDK, Offerings дозволяють
A/B ціни без релізу. На демо — мок, у проді — реальна транзакція за 1 виклик."

## Час
- Демо мок: ~10 хв (Zustand + reuse paywall)
- Прод RevenueCat: 1-2 год + налаштування сторів (НЕ для буткемпу)
