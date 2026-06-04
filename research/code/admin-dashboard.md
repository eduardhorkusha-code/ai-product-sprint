# Admin Dashboard — "повноцінний продукт" для суддів-будівників
**Дата:** 2026-06-04
**Питання Eduard:** чи варто будувати адмін-панель щоб виглядало як повний продукт?

---

## ВІДПОВІДЬ: ТАК — але LIGHTWEIGHT mock, не повна система

### Чому ТАК
- SKELAR-судді думають **всім бізнесом**: user app + admin + метрики + воронка. Адмін-вʼю
  сигналить "я збудував БІЗНЕС, а не фічу" — це потужний диференціатор.
- "Бачити більше / працювати з клієнтами" = погляд оператора/менеджера = саме їхня оптика.
- Більшість команд покажуть тільки user-екран. Адмін-панель = ти на рівень вище.
- **Реюз:** наш Team Energy Pulse дашборд УЖЕ є admin/manager-вʼю. Воронка дає funnel-метрики.
  Це не нова система — це левел-ап того, що вже є.

### Чому LIGHTWEIGHT (не повна)
- ⏱️ 4 години. Повна функціональна адмінка = друга поверхня = розмиває фокус.
- Winning formula = ОДИН wow + Social Proof. Адмінка не має красти центр.
- Напівзроблена адмінка гірша за відсутню.

### Правило
**~20-30 хв. Один екран. Mock-метрики + 1-2 реальні (live vote count з Supabase).**
Будуй ТІЛЬКИ якщо: core+funnel готові рано, АБО тема = admin/ops/management/CRM.
Інакше — пропусти, не ризикуй центром демо.

---

## Які метрики накидати (стандарт SaaS, що впізнають судді)

| Блок | Метрики | Real чи mock |
|------|---------|--------------|
| KPI cards | MRR, Active users (DAU/WAU), Trial→Paid %, Churn | mock (реалістичні) |
| Воронка | Visit → Quiz start → Quiz complete → Trial → Paid (з %) | mock + funnel логіка |
| Графік | MRR trend / Energy trend 7 днів | mock крива |
| Live | Голоси/активність зараз | ✅ REAL з Supabase |
| Клієнти | Список останніх користувачів + статус | mock + кілька real |

Реалістичні mock-значення (щоб не виглядало фейково):
```
MRR ₴248,500 · Active 1,847 · Trial→Paid 42% · Churn 3.2%
Воронка: 10,000 → 6,800 (68%) → 4,200 (62%) → 1,950 (46%) → 820 (42%)
```

---

## Код — `app/(tabs)/admin.tsx`
```tsx
import { View, Text, ScrollView } from 'react-native'
import { LineGraph } from 'react-native-graph'
import { usePulse, getSessionId } from '@/lib/usePulse'
import { useEffect, useState } from 'react'
import { SESSION_CODE } from '@/lib/energy'

// Mock метрики (реалістичні)
const KPIS = [
  { label: 'MRR', value: '₴248.5K', delta: '+12%', up: true },
  { label: 'Active', value: '1,847', delta: '+5%', up: true },
  { label: 'Trial→Paid', value: '42%', delta: '+3pp', up: true },
  { label: 'Churn', value: '3.2%', delta: '-0.4pp', up: true },
]
const FUNNEL = [
  { stage: 'Visit', n: 10000, pct: 100 },
  { stage: 'Quiz start', n: 6800, pct: 68 },
  { stage: 'Quiz complete', n: 4200, pct: 62 },
  { stage: 'Trial', n: 1950, pct: 46 },
  { stage: 'Paid', n: 820, pct: 42 },
]
const MRR_POINTS = [180, 195, 210, 218, 232, 240, 248].map((v, i) => ({
  date: new Date(2026, 5, i + 1), value: v,
}))

function Card({ label, value, delta, up }: typeof KPIS[number]) {
  return (
    <View style={{ flex: 1, minWidth: '45%', backgroundColor: '#15151b', borderRadius: 16, padding: 16 }}>
      <Text style={{ color: '#9ca3af', fontSize: 13 }}>{label}</Text>
      <Text style={{ color: '#fff', fontSize: 24, fontWeight: '800', marginTop: 4 }}>{value}</Text>
      <Text style={{ color: up ? '#22c55e' : '#ef4444', fontSize: 13, marginTop: 4 }}>{delta}</Text>
    </View>
  )
}

export default function Admin() {
  const [sid, setSid] = useState<string | null>(null)
  useEffect(() => { getSessionId(SESSION_CODE).then(setSid) }, [])
  const { count } = usePulse(sid)  // REAL live число

  return (
    <ScrollView style={{ flex: 1, backgroundColor: '#0b0b0f' }} contentContainerStyle={{ padding: 20 }}>
      <Text style={{ color: '#fff', fontSize: 26, fontWeight: '800' }}>Admin</Text>
      <Text style={{ color: '#9ca3af', marginTop: 4 }}>Огляд бізнесу · {count} активних зараз (live)</Text>

      {/* KPI */}
      <View style={{ flexDirection: 'row', flexWrap: 'wrap', gap: 12, marginTop: 20 }}>
        {KPIS.map((k) => <Card key={k.label} {...k} />)}
      </View>

      {/* MRR графік */}
      <Text style={{ color: '#fff', fontSize: 16, fontWeight: '700', marginTop: 28 }}>MRR · 7 днів</Text>
      <View style={{ height: 160, marginTop: 12 }}>
        <LineGraph points={MRR_POINTS} animated color="#6366f1" style={{ flex: 1 }} />
      </View>

      {/* Воронка */}
      <Text style={{ color: '#fff', fontSize: 16, fontWeight: '700', marginTop: 28 }}>Воронка конверсії</Text>
      <View style={{ marginTop: 12, gap: 8 }}>
        {FUNNEL.map((f) => (
          <View key={f.stage}>
            <View style={{ flexDirection: 'row', justifyContent: 'space-between', marginBottom: 4 }}>
              <Text style={{ color: '#d1d5db', fontSize: 14 }}>{f.stage}</Text>
              <Text style={{ color: '#9ca3af', fontSize: 14 }}>{f.n.toLocaleString()} · {f.pct}%</Text>
            </View>
            <View style={{ height: 10, backgroundColor: '#1f2937', borderRadius: 5 }}>
              <View style={{ height: 10, width: `${f.pct}%`, backgroundColor: '#6366f1', borderRadius: 5 }} />
            </View>
          </View>
        ))}
      </View>
    </ScrollView>
  )
}
```

---

## Демо-сила
У пітчі переходиш з user-екрану на admin-таб:
> "А ось як це бачить бізнес: MRR, воронка з 42% trial→paid, активні зараз — live.
> Це не фіча, це продукт з unit-економікою."

Судді-будівники чують свою мову → "розуміє як працює бізнес".

## Адаптація
Метрики — стандарт SaaS, підходять під будь-який subscription-продукт. Міняєш цифри під
тему. Воронка тягнеться з onboarding-funnel.md (ті ж етапи). Live-число — з usePulse.
