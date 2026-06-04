# Starter Kit — копіюй у день X

Готові до вставки артефакти. На буткемпі: клонуєш шаблон → копіюєш звідси → будуєш фічі.
(Код тримається в markdown-блоках, бо це "копіюй-встав", не живі source-файли.)

---

## 1. `.env.local` (корінь застосунку)
Бери з [.env.example](.env.example). Реальні значення:
- Supabase: app.supabase.com → Project Settings → API
- Claude: console.anthropic.com → API Keys

---

## 2. `lib/supabase.ts` — безпечний клієнт (SecureStore)
```bash
npx expo install @supabase/supabase-js expo-secure-store react-native-url-polyfill
```
```typescript
import 'react-native-url-polyfill/auto'
import * as SecureStore from 'expo-secure-store'
import { createClient } from '@supabase/supabase-js'

const ExpoSecureStoreAdapter = {
  getItem: (key: string): Promise<string | null> => SecureStore.getItemAsync(key),
  setItem: (key: string, value: string): Promise<void> => SecureStore.setItemAsync(key, value),
  removeItem: (key: string): Promise<void> => SecureStore.deleteItemAsync(key),
}

export const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL as string,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY as string,
  {
    auth: {
      storage: ExpoSecureStoreAdapter as any,
      autoRefreshToken: true,
      persistSession: true,
      detectSessionInUrl: false,
    },
  }
)
```

---

## 3. Supabase schema — встав у SQL Editor
Покриває 3 топ-ідеї (habits / moods / wins). Видали зайве під обраний продукт.
```sql
-- HABITS (Habit Stack)
create table public.habits (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references auth.users,
  name text not null, emoji text default '✅', color text default '#6366f1',
  is_done boolean not null default false,
  created_at timestamptz not null default now()
);
create index on public.habits(user_id);

create table public.habit_logs (
  id uuid primary key default gen_random_uuid(),
  habit_id uuid not null references public.habits(id) on delete cascade,
  user_id uuid not null references auth.users,
  completed_date date not null,
  created_at timestamptz not null default now(),
  unique(habit_id, completed_date)
);
create index on public.habit_logs(user_id, completed_date);

alter table public.habits enable row level security;
alter table public.habit_logs enable row level security;
create policy "own habits" on public.habits for all to authenticated
  using (auth.uid() = user_id) with check (auth.uid() = user_id);
create policy "own logs" on public.habit_logs for all to authenticated
  using (auth.uid() = user_id) with check (auth.uid() = user_id);

-- MOODS (Team Pulse) — анонімне realtime голосування
create table public.moods (
  id uuid primary key default gen_random_uuid(),
  device_id text not null, emoji text not null,
  score int not null check (score between 1 and 5),
  session_id text not null,
  created_at timestamptz not null default now()
);
alter table public.moods enable row level security;
create policy "anyone insert mood" on public.moods for insert to anon, authenticated with check (true);
create policy "anyone read moods" on public.moods for select to anon, authenticated using (true);
alter publication supabase_realtime add table public.moods;  -- Social Proof Moment!

-- WINS (Win Journal)
create table public.wins (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references auth.users,
  text text not null, color text default '#6366f1',
  created_at timestamptz not null default now()
);
create index on public.wins(user_id, created_at desc);
alter table public.wins enable row level security;
create policy "own wins" on public.wins for all to authenticated
  using (auth.uid() = user_id) with check (auth.uid() = user_id);
```

---

## 4. `CLAUDE.md` застосунку (Expo-специфічний)
Встав у корінь застосунку щоб Claude Code писав точно.
```markdown
# [ProjectName] — [1 речення]

## Стек
- Expo SDK 53 + Expo Router v3 (typedRoutes: true)
- NativeWind v4.2.1+ (НЕ 4.2.0) · Reanimated + Gesture Handler
- Zustand + AsyncStorage · Supabase (auth через SecureStore)
- TypeScript strict

## Команди (Bun, ЗАБОРОНЕНО npm/yarn)
- bun install · bunx expo start · bun run typecheck (tsc --noEmit)

## Архітектурні правила
- Routing: тільки typedRoutes Expo Router v3
- Auth: токени ВИКЛЮЧНО через SecureStore adapter (lib/supabase.ts)
- Стилі: NativeWind класи, ЗАБОРОНЕНО inline StyleSheet
- IDs: nanoid/non-secure · Дати: toISOString().split('T')[0]
- Lists: FlashList + estimatedItemSize, не FlatList
- Spacing: multiples of 4 · Max 3 font sizes (22/17/13)
- 'use dom' компоненти: тільки серіалізовані пропси

## Не будувати
- Push notifications → in-app banner (research in-app-feedback.md)
- OAuth → local device id або Supabase email
- Анімація >30хв → бібліотека

## RN performance (Callstack)
Читай reference/callstack-rn-best-practices/SKILL.md:
- НЕ useMemo/useCallback без profiler · direct imports не barrel
- Reanimated shared values, не useState в анімаціях

## Verify перед commit
bun run typecheck — 0 помилок
```

---

## 5. Метро-фікс для Supabase (SDK 53)
`metro.config.js`:
```js
const { getDefaultConfig } = require('expo/metro-config')
const { withNativeWind } = require('nativewind/metro')
const config = getDefaultConfig(__dirname)
config.resolver.unstable_enablePackageExports = false  // Supabase ES Module fix
module.exports = withNativeWind(config, { input: './app/globals.css' })
```
