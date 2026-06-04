# AI Engineer — Experience Log (Vision)

Уроки накопичуються тут. Читай на початку кожної задачі, допиши в кінці.

## 2026-06-04 — seed з research (до буткемпу)
- Модель для демо: claude-haiku-4-5 (швидкість > глибина, ~800мс). Складне → sonnet.
- Streaming через Vercel AI SDK (ai + @ai-sdk/anthropic), код у research/code/ai-features.md.
- Security: EXPO_PUBLIC_ANTHROPIC_API_KEY OK для hackathon, НІКОЛИ для prod (декомпілюється).
- Structured output: проси JSON, парси з try/catch + fallback. Порожньо → graceful, не "помилка".
- ТЕМНА ПЛЯМА #1 (research/STATUS.md): voice→text транскрипція не закрита. Whisper API vs on-device — запустити промпт зі STATUS.md перш ніж будувати Meeting Action Extractor.
- Принцип: AI що ДІЄ (voice→action) виграє standing ovation, AI що summarizes — ввічливі оплески.
- Latency UX: завжди спіннер/прогрес під час чекання, не чорний екран.
