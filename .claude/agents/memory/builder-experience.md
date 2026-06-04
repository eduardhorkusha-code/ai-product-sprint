# Builder — Experience Log (Tony Stark)

Уроки накопичуються тут. Читай на початку кожної задачі, допиши в кінці.

## 2026-06-04 — донавчання: готові скелети (copy-paste, не пиши з нуля)
- ⏱️ 4 ГОДИНИ. PLAYBOOK.md = план. Реюз > писати з нуля.
- **Team Energy Pulse БЕКАП** — starter/BACKUP-team-energy-pulse.md: повний код (schema+seed, usePulse realtime хук, vote-екран, дашборд зі score-кільцем). БД стоїть з prep. Якщо тема схожа → copy-paste.
- **Onboarding funnel** — code/onboarding-funnel.md: quiz store, QuizStep, BuildingProfile, Result з прогноз-графіком, Paywall. ~30-40хв.
- **Admin dashboard** — code/admin-dashboard.md: РЕЮЗ того ж usePulse + LineGraph. Mock KPI/воронка + 1 real live-число. ~20-30хв.
- Ключове: vote-дашборд, admin, funnel-result ВСІ використовують той самий усеReanimated score-ring + LineGraph. Один патерн візуалізації → три екрани.
- charts.md: react-native-graph (Skia) потребує dev build на деяких; svg progress ring працює в чистому Expo Go. QA перевіряє на пристрої.

## 2026-06-04 — seed з research (до буткемпу)
- Шаблон: toyamarodrigo/expo-router-template (FREE MIT, 5хв setup). Zustand + TanStack вже є.
- NativeWind ТІЛЬКИ 4.2.1+ (4.2.0 зламана — ref губиться, Babel конфлікт).
- Supabase + SDK 53: metro.config `unstable_enablePackageExports = false` (ES Module fix).
- Auth ТІЛЬКИ через expo-secure-store adapter, НЕ AsyncStorage (код у research/code/supabase-schemas.md).
- FlashList завжди (не FlatList) + estimatedItemSize обов'язковий. sharedValue скидати при recycle: useEffect(() => { x.value = 0 }, [item.id]).
- IDs: nanoid/non-secure. Haptics на кожній інтерактивній дії.
- Nuclear fix 80% проблем: rm -rf node_modules .expo && bun install && expo start --clear.
- Callstack agent-skills: НЕ useMemo/useCallback без profiler, direct imports не barrel, Reanimated shared values не useState в анімаціях.
