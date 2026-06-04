# Builder — Experience Log (Tony Stark)

Уроки накопичуються тут. Читай на початку кожної задачі, допиши в кінці.

## 2026-06-04 — seed з research (до буткемпу)
- Шаблон: toyamarodrigo/expo-router-template (FREE MIT, 5хв setup). Zustand + TanStack вже є.
- NativeWind ТІЛЬКИ 4.2.1+ (4.2.0 зламана — ref губиться, Babel конфлікт).
- Supabase + SDK 53: metro.config `unstable_enablePackageExports = false` (ES Module fix).
- Auth ТІЛЬКИ через expo-secure-store adapter, НЕ AsyncStorage (код у research/code/supabase-schemas.md).
- FlashList завжди (не FlatList) + estimatedItemSize обов'язковий. sharedValue скидати при recycle: useEffect(() => { x.value = 0 }, [item.id]).
- IDs: nanoid/non-secure. Haptics на кожній інтерактивній дії.
- Nuclear fix 80% проблем: rm -rf node_modules .expo && bun install && expo start --clear.
- Callstack agent-skills: НЕ useMemo/useCallback без profiler, direct imports не barrel, Reanimated shared values не useState в анімаціях.
