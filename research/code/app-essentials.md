# Module: App Essentials — notifications, analytics, profile, permissions
**Дата:** 2026-06-04
**Дрібні цеглинки що роблять продукт "повним". Бери вибірково.**

---

## 1. Push Notifications (~15 хв, але на демо — fake)
⚠️ Реальні push потребують dev build + конфіг. На демо → in-app banner
([in-app-feedback.md](in-app-feedback.md)). Прод-версія:
```bash
npx expo install expo-notifications
```
```typescript
import * as Notifications from 'expo-notifications'

export async function scheduleReminder(title: string, body: string, seconds: number) {
  await Notifications.requestPermissionsAsync()
  await Notifications.scheduleNotificationAsync({
    content: { title, body },
    trigger: { seconds },
  })
}
```
Демо-трюк: in-app banner по таймеру виглядає як push (in-app-feedback.md).

---

## 2. Analytics Events (~10 хв) — SKELAR це ЛЮБИТЬ
Трекати воронку = мова суддів. Простий трекер у Supabase (або console на демо).
```typescript
// lib/track.ts
import { supabase } from './supabase'

export function track(event: string, props: Record<string, any> = {}) {
  console.log('[track]', event, props)            // на демо видно в логах
  supabase.from('events').insert({ event, props }).then(() => {})  // опц. у БД
}
```
```sql
create table public.events (
  id uuid primary key default gen_random_uuid(),
  event text not null, props jsonb default '{}',
  created_at timestamptz not null default now()
);
alter table public.events enable row level security;
create policy "insert events" on public.events for insert to anon, authenticated with check (true);
```
Ключові події воронки: `quiz_start`, `quiz_complete`, `paywall_view`, `trial_start`, `subscribe`.
→ Ці події живлять admin-dashboard воронку (реальні числа замість mock).

---

## 3. Profile / Settings (~15 хв)
```tsx
import { View, Text, Pressable } from 'react-native'
import { useSubscription } from '@/lib/subscription'
import { signOut } from '@/lib/auth'

export default function Profile() {
  const { isPro, plan, reset } = useSubscription()
  return (
    <View style={{ flex: 1, padding: 24, backgroundColor: '#fff' }}>
      <Text style={{ fontSize: 26, fontWeight: '800', marginTop: 40 }}>Профіль</Text>

      <View style={{ marginTop: 24, padding: 18, borderRadius: 16, backgroundColor: '#f9fafb' }}>
        <Text style={{ fontSize: 13, color: '#6b7280' }}>План</Text>
        <Text style={{ fontSize: 18, fontWeight: '700', marginTop: 4 }}>
          {isPro ? `Pro · ${plan}` : 'Free'}
        </Text>
      </View>

      {[
        { label: 'Сповіщення', emoji: '🔔' },
        { label: 'Приватність', emoji: '🔒' },
        { label: 'Підтримка', emoji: '💬' },
      ].map((row) => (
        <Pressable key={row.label}
          style={{ flexDirection: 'row', alignItems: 'center', gap: 12, paddingVertical: 16,
            borderBottomWidth: 1, borderBottomColor: '#f3f4f6' }}>
          <Text style={{ fontSize: 20 }}>{row.emoji}</Text>
          <Text style={{ fontSize: 16, flex: 1 }}>{row.label}</Text>
          <Text style={{ color: '#d1d5db' }}>›</Text>
        </Pressable>
      ))}

      <Pressable onPress={signOut} style={{ marginTop: 32 }}>
        <Text style={{ color: '#ef4444', fontSize: 16 }}>Вийти</Text>
      </Pressable>
    </View>
  )
}
```

---

## 4. Permission Priming (~10 хв) — конверсія дозволів
Не проси дозвіл системним діалогом одразу — спочатку поясни ЦІННІСТЬ (soft ask),
потім системний (hard ask). Підвищує opt-in.
```tsx
// Soft ask екран ПЕРЕД Notifications.requestPermissionsAsync()
<View>
  <Text style={{ fontSize: 24, fontWeight: '800' }}>Нагадаємо у правильний час</Text>
  <Text style={{ color: '#6b7280', marginTop: 8 }}>
    Розумні нагадування коли твоя енергія падає. Без спаму.
  </Text>
  <Pressable onPress={askSystemPermission}>
    <Text>Увімкнути нагадування</Text>
  </Pressable>
  <Pressable onPress={skip}><Text>Пізніше</Text></Pressable>
</View>
```

---

## Що брати на демо
| Цеглинка | Демо? |
|----------|-------|
| Analytics events | ✅ ТАК — живить admin воронку реальними числами + мова суддів |
| Profile/Settings | ✅ якщо є час — "повний продукт" |
| Permission priming | 🟡 якщо продукт про нагадування |
| Real push | ❌ ні (dev build) → fake banner |
