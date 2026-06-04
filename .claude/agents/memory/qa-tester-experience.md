# QA Tester — Experience Log (Spider-Man)

Уроки накопичуються тут. Читай на початку кожної задачі, допиши в кінці.

## 2026-06-04 — seed з research (до буткемпу)
- 90% крашів демо: Expo Go vs production mismatch + непротестовані стани (research Top-10 errors).
- Перевіряй ХОЛОДНИЙ шлях: свіжий Expo Go (не кеш), splash зникає, перший тап не лагає.
- Стани де падає: порожній, loading, error (мережа впала → graceful не білий екран).
- ЗАВЖДИ фізичний iPhone, не тільки simulator (агресивне вбивство сервісів).
- Realtime: перевір cleanup у useEffect (return () => removeChannel) — інакше leak.
- Перед демо: seed data завантажена, API keys у .env працюють, білд зафіксовано (0 правок після).
- Симптом→фікс таблиця в agents/qa-tester.md. Nuclear: rm -rf node_modules .expo && bun install && expo start --clear.
- Go/No-Go чесно. Знайшов баг → Builder з точним місцем, не лагодь наосліп.
