# BACKUP BASE — Team Energy Pulse (готовий скелет)

> 🎯 Наш найімовірніший продукт + найадаптивніший патерн. Розгортаєш ЗАВТРА (prep),
> у суботу він уже працює — лишається адаптувати під реальне завдання.
>
> **Патерн = анонімний realtime-збір → живий агрегат-дашборд.** Адаптується під майже
> будь-яку схожу тему: енергія/настрій команди, фідбек, live-голосування, опитування,
> stand-up check-in, Q&A. Якщо тема схожа — це твоя основа. Якщо ні — викидаєш за 2 хв.

---

## Чому саме це (перетин факторів)
- ✅ Social Proof Moment ВБУДОВАНИЙ (аудиторія шле → екран оживає live)
- ✅ Резонує з тезою організатора "Енергія = NEW GOLD" + "керівник = точка опори"
- ✅ Без auth → економить годину
- ✅ realtime = весь wow, core ~75хв
- ✅ Патерн адаптується під багато тем

---

## 📋 ЗАВТРА (prep) — розгорнути за 20 хв

1. **Supabase проект** (app.supabase.com → New Project, запам'ятай DB password)
2. **SQL Editor → встав схему нижче** (§ Schema) → Run
3. **Скопіюй ключі** (Settings → API): URL + anon key → у `.env.local`
4. **Перевір realtime** (Database → Replication → supabase_realtime має містити pulse_votes)
5. **Залиш проект готовим.** У суботу БД уже стоїть — нуль витрат часу.

Мінімальні креди (більше нічого не треба для core):
```
EXPO_PUBLIC_SUPABASE_URL=...
EXPO_PUBLIC_SUPABASE_ANON_KEY=...
```
(AI не потрібен для core. Якщо додаси AI-інсайт — тоді ще ANTHROPIC key.)

---

## 🗄️ Schema (встав у Supabase SQL Editor)
```sql
-- Сесія пульсу (stand-up / зустріч). Код для приєднання.
create table public.pulse_sessions (
  id uuid primary key default gen_random_uuid(),
  name text not null default 'Team Pulse',
  code text unique not null,                  -- короткий join-код, напр. 'TEAM'
  created_at timestamptz not null default now()
);

-- Голоси енергії (анонімні)
create table public.pulse_votes (
  id uuid primary key default gen_random_uuid(),
  session_id uuid not null references public.pulse_sessions(id) on delete cascade,
  device_id text not null,
  emoji text not null,
  label text not null,
  score int not null check (score between 1 and 5),
  created_at timestamptz not null default now()
);
create index on public.pulse_votes(session_id, created_at);

-- RLS: анонімний доступ (без auth)
alter table public.pulse_sessions enable row level security;
alter table public.pulse_votes enable row level security;
create policy "read sessions" on public.pulse_sessions for select to anon, authenticated using (true);
create policy "create session" on public.pulse_sessions for insert to anon, authenticated with check (true);
create policy "read votes"  on public.pulse_votes for select to anon, authenticated using (true);
create policy "cast vote"   on public.pulse_votes for insert to anon, authenticated with check (true);

-- Realtime (Social Proof Moment!)
alter publication supabase_realtime add table public.pulse_votes;

-- Демо-сесія + seed (щоб дашборд не був порожній на старті демо)
insert into public.pulse_sessions (name, code) values ('SKELAR Bootcamp', 'SKLR');
-- кілька seed-голосів (заміни session_id після створення, або через підзапит):
insert into public.pulse_votes (session_id, device_id, emoji, label, score)
select id, 'seed-'||g, e.emoji, e.label, e.score
from public.pulse_sessions s,
     (values ('🚀','Палаю',5),('😄','Драйв',4),('🙂','Норм',3),('😄','Драйв',4),('🚀','Палаю',5)) as e(emoji,label,score),
     generate_series(1,1) g
where s.code = 'SKLR';
```

---

## 🎨 Код застосунку (копіюй у день X)

> `lib/supabase.ts` — бери зі [STARTER.md](STARTER.md) §2 (SecureStore клієнт).

### `lib/energy.ts` — опції + типи
```typescript
export const ENERGY_OPTIONS = [
  { emoji: '😴', label: 'Вигорів', score: 1, color: '#ef4444' },
  { emoji: '😐', label: 'Втомлений', score: 2, color: '#f59e0b' },
  { emoji: '🙂', label: 'Норм', score: 3, color: '#eab308' },
  { emoji: '😄', label: 'Драйв', score: 4, color: '#22c55e' },
  { emoji: '🚀', label: 'Палаю', score: 5, color: '#6366f1' },
] as const

export interface Vote {
  id: string; session_id: string; device_id: string
  emoji: string; label: string; score: number; created_at: string
}

export const SESSION_CODE = 'SKLR'  // демо-сесія
```

### `lib/usePulse.ts` — realtime хук (серце wow)
```typescript
import { useEffect, useState } from 'react'
import { supabase } from './supabase'
import type { Vote } from './energy'

export function usePulse(sessionId: string | null) {
  const [votes, setVotes] = useState<Vote[]>([])

  useEffect(() => {
    if (!sessionId) return
    // початкові дані
    supabase.from('pulse_votes').select('*')
      .eq('session_id', sessionId)
      .then(({ data }) => { if (data) setVotes(data as Vote[]) })

    // realtime підписка
    const channel = supabase
      .channel(`pulse-${sessionId}`)
      .on('postgres_changes',
        { event: 'INSERT', schema: 'public', table: 'pulse_votes', filter: `session_id=eq.${sessionId}` },
        (payload) => setVotes((prev) => [...prev, payload.new as Vote]))
      .subscribe()

    return () => { supabase.removeChannel(channel) }  // cleanup — критично!
  }, [sessionId])

  const avg = votes.length ? votes.reduce((s, v) => s + v.score, 0) / votes.length : 0
  const energyPct = Math.round((avg / 5) * 100)
  const distribution = [1, 2, 3, 4, 5].map((s) => votes.filter((v) => v.score === s).length)

  return { votes, count: votes.length, energyPct, distribution }
}

// Допоміжне: знайти/створити сесію за кодом
export async function getSessionId(code: string): Promise<string | null> {
  const { data } = await supabase.from('pulse_sessions').select('id').eq('code', code).single()
  return data?.id ?? null
}
```

### `app/(tabs)/vote.tsx` — екран голосування (для аудиторії)
```tsx
import { useState, useEffect } from 'react'
import { View, Text, Pressable } from 'react-native'
import * as Haptics from 'expo-haptics'
import { ENERGY_OPTIONS, SESSION_CODE } from '@/lib/energy'
import { supabase } from '@/lib/supabase'
import { getSessionId } from '@/lib/usePulse'
import { nanoid } from 'nanoid/non-secure'

const DEVICE_ID = nanoid()  // анонімний id пристрою

export default function Vote() {
  const [sessionId, setSessionId] = useState<string | null>(null)
  const [voted, setVoted] = useState(false)

  useEffect(() => { getSessionId(SESSION_CODE).then(setSessionId) }, [])

  const cast = async (opt: typeof ENERGY_OPTIONS[number]) => {
    if (!sessionId) return
    Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success)
    await supabase.from('pulse_votes').insert({
      session_id: sessionId, device_id: DEVICE_ID,
      emoji: opt.emoji, label: opt.label, score: opt.score,
    })
    setVoted(true)
  }

  if (voted) return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center', backgroundColor: '#fff' }}>
      <Text style={{ fontSize: 64 }}>✅</Text>
      <Text style={{ fontSize: 22, fontWeight: '700', marginTop: 16 }}>Дякуємо!</Text>
      <Text style={{ color: '#6b7280', marginTop: 8 }}>Твоя енергія врахована</Text>
    </View>
  )

  return (
    <View style={{ flex: 1, padding: 24, justifyContent: 'center', backgroundColor: '#fff' }}>
      <Text style={{ fontSize: 26, fontWeight: '800', textAlign: 'center', marginBottom: 8 }}>
        Яка твоя енергія?
      </Text>
      <Text style={{ fontSize: 16, color: '#6b7280', textAlign: 'center', marginBottom: 32 }}>
        Анонімно. Один тап.
      </Text>
      {ENERGY_OPTIONS.map((opt) => (
        <Pressable key={opt.score} onPress={() => cast(opt)}
          style={{ flexDirection: 'row', alignItems: 'center', gap: 16,
            padding: 18, borderRadius: 16, marginBottom: 12,
            borderWidth: 2, borderColor: opt.color + '40' }}>
          <Text style={{ fontSize: 32 }}>{opt.emoji}</Text>
          <Text style={{ fontSize: 18, fontWeight: '600' }}>{opt.label}</Text>
        </Pressable>
      ))}
    </View>
  )
}
```

### `app/(tabs)/index.tsx` — живий дашборд (для менеджера = wow-екран)
```tsx
import { useEffect, useState } from 'react'
import { View, Text } from 'react-native'
import Animated, { useSharedValue, useAnimatedProps, withTiming } from 'react-native-reanimated'
import Svg, { Circle } from 'react-native-svg'
import { ENERGY_OPTIONS, SESSION_CODE } from '@/lib/energy'
import { usePulse, getSessionId } from '@/lib/usePulse'

const AnimatedCircle = Animated.createAnimatedComponent(Circle)

export default function Dashboard() {
  const [sessionId, setSessionId] = useState<string | null>(null)
  useEffect(() => { getSessionId(SESSION_CODE).then(setSessionId) }, [])
  const { count, energyPct, distribution } = usePulse(sessionId)

  // анімоване score-кільце (Whoop-style)
  const size = 200, radius = 88, circ = 2 * Math.PI * radius
  const p = useSharedValue(0)
  useEffect(() => { p.value = withTiming(energyPct / 100, { duration: 600 }) }, [energyPct])
  const ringProps = useAnimatedProps(() => ({ strokeDashoffset: circ * (1 - p.value) }))
  const color = energyPct >= 70 ? '#22c55e' : energyPct >= 40 ? '#eab308' : '#ef4444'

  return (
    <View style={{ flex: 1, backgroundColor: '#0b0b0f', padding: 24, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: '#9ca3af', fontSize: 16, marginBottom: 4 }}>Енергія команди зараз</Text>
      <Text style={{ color: '#fff', fontSize: 13, marginBottom: 24 }}>{count} голосів • live</Text>

      <View style={{ width: size, height: size, alignItems: 'center', justifyContent: 'center' }}>
        <Svg width={size} height={size} style={{ position: 'absolute', transform: [{ rotate: '-90deg' }] }}>
          <Circle cx={size/2} cy={size/2} r={radius} stroke="#1f2937" strokeWidth={14} fill="none" />
          <AnimatedCircle cx={size/2} cy={size/2} r={radius} stroke={color} strokeWidth={14}
            fill="none" strokeDasharray={circ} strokeLinecap="round" animatedProps={ringProps} />
        </Svg>
        <Text style={{ color: '#fff', fontSize: 48, fontWeight: '800' }}>{energyPct}%</Text>
      </View>

      {/* розподіл */}
      <View style={{ flexDirection: 'row', gap: 12, marginTop: 32 }}>
        {ENERGY_OPTIONS.map((opt, i) => (
          <View key={opt.score} style={{ alignItems: 'center' }}>
            <Text style={{ fontSize: 24 }}>{opt.emoji}</Text>
            <Text style={{ color: '#fff', fontSize: 18, fontWeight: '700', marginTop: 4 }}>
              {distribution[i]}
            </Text>
          </View>
        ))}
      </View>
    </View>
  )
}
```

**Залежності:**
```bash
npx expo install react-native-reanimated react-native-svg expo-haptics \
  @supabase/supabase-js expo-secure-store react-native-url-polyfill nanoid
```

---

## 🎬 Social Proof Moment (демо)
1. Дашборд відкритий на твоєму екрані (через iPhone Mirroring / web-лінк).
2. Кажеш: **"Всі відкрийте vote-екран і шліть свою енергію — дивимось live."**
3. Голоси прилітають → кільце росте/змінює колір, лічильник +1. WOW.
4. CTA: "За 4 години — інструмент що дає менеджеру пульс команди за 10 секунд."

---

## 🔀 Як АДАПТУВАТИ якщо тема інша (але схожа)
Патерн той самий — міняється тільки `ENERGY_OPTIONS` + рамка:

| Якщо тема... | Зміни |
|--------------|-------|
| Фідбек на щось | options → категорії фідбеку, дашборд → топ-категорії |
| Live-голосування/опитування | options → варіанти відповіді, кільце → % за лідера |
| Stand-up check-in | додай text-поле у vote, дашборд → стрічка |
| Q&A / питання | таблиця questions + upvotes, дашборд → топ питань |
| Настрій/задоволеність | майже як є, зміни лейбли |

Ядро (schema realtime + usePulse + score-ring дашборд) НЕ чіпаєш — воно reusable.
Якщо тема ЗОВСІМ інша → INDEX.md фаза 0, обираєш заново, цей бекап ігноруєш.
