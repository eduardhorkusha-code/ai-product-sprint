# 🔑 SECRETS — ключі та env (день X)

Майстер-лист: де взяти кожен ключ, куди вставити, який формат. Архітектура — **secure proxy**: ключі живуть у Vercel, застосунок ходить у Vercel, не напряму в Anthropic.

```
Expo App ──→ Vercel Function (/api/claude) ──→ Claude API
   │              ↑ тут секретні ключі
   └──→ Supabase (anon key — публічний, захищений RLS)
```

Готовий proxy для копіювання: [starter/VERCEL-AI-PROXY.md](starter/VERCEL-AI-PROXY.md).

---

## ТАБЛИЦЯ 1 — Де взяти

| # | Сервіс | Пряме посилання | Що натиснути | Формат значення |
|---|--------|-----------------|--------------|-----------------|
| 1 🔴 | **Anthropic** | https://console.anthropic.com/settings/keys | "Create Key" | `sk-ant-api03-…` |
| 2a 🟡 | **Supabase** URL | https://supabase.com/dashboard → проєкт → Settings → API | блок "Project URL" | `https://xxxx.supabase.co` |
| 2b 🟡 | **Supabase** anon | там само → Project API keys → `anon` `public` | copy | `eyJhbGci…` (JWT) |
| 2c 🟡 | **Supabase** service | там само → `service_role` `secret` → Reveal | copy | `eyJhbGci…` (інший JWT) |
| 3 🟢 | **OpenAI** (voice) | https://platform.openai.com/api-keys | "Create new secret key" | `sk-proj-…` |
| 4 ⚪ | **ElevenLabs** (TTS) | https://elevenlabs.io/app/settings/api-keys | "Create API Key" | `sk_…` |

> 🔴 спершу: чи є **спільний SKELAR Anthropic workspace**? Спитай #platform_ai / IT — щоб не платити з власної картки. Claude.ai Team підписка ≠ API доступ.
>
> Supabase у нових проєктах: ключі можуть зватись `sb_publishable_…` / `sb_secret_…` — це те саме що `anon` / `service_role`.

---

## ТАБЛИЦЯ 2 — Куди вставити

### 🔒 Vercel — Project → Settings → Environment Variables (всі 3 середовища)
| Поле (name) | Значення | Заповнено |
|-------------|----------|:---------:|
| `ANTHROPIC_API_KEY` | #1 | ☐ |
| `CLAUDE_MODEL` | `claude-opus-4-8` (або `claude-haiku-4-5` для швидкого демо) | ☐ |
| `OPENAI_API_KEY` | #3 (опц.) | ☐ |
| `ELEVENLABS_API_KEY` | #4 (опц.) | ☐ |

### 📱 Expo — `.env.local` у корені застосунку
| Поле (name) | Значення | Заповнено |
|-------------|----------|:---------:|
| `EXPO_PUBLIC_SUPABASE_URL` | #2a | ☐ |
| `EXPO_PUBLIC_SUPABASE_ANON_KEY` | #2b | ☐ |
| `EXPO_PUBLIC_API_URL` | URL Vercel-проєкту після деплою (`https://…vercel.app`) | ☐ |

### 🗄️ Supabase — нікуди не вставляєш, використовується в коді
`service_role` (#2c) — лише якщо Vercel-функція пише в БД bypass RLS. На демо зазвичай не треба.

---

## ПРАВИЛА БЕЗПЕКИ

| ✅ Можна | ❌ Не можна |
|----------|-------------|
| `EXPO_PUBLIC_SUPABASE_*` у клієнті (anon захищений RLS) | `ANTHROPIC_API_KEY` у клієнті / коді / git |
| `EXPO_PUBLIC_API_URL` (це просто адреса) | `EXPO_PUBLIC_` префікс на будь-якому `sk-…` ключі |
| Секретні `sk-…` ключі — тільки у Vercel env | Комітити `.env.local` (має бути в `.gitignore`) |

**Чому:** `EXPO_PUBLIC_` потрапляє в JS-bundle застосунку → будь-хто розпакує APK і дістане ключ → платиш за чужі запити.

Після правки `.env.local`: рестарт `npx expo start -c` (інакше старий кеш env).

---

## ⚡ Швидкий fallback (якщо немає часу на Vercel)
ai-features.md має direct-call патерн з `EXPO_PUBLIC_ANTHROPIC_API_KEY`. Працює, але **ключ витікає**. Тільки якщо: (а) горить дедлайн і (б) ключ потім видалиш/зротуєш одразу після демо. Proxy = +20 хв, але правильно.

---

## ✅ Мінімум для перемоги
🔴 Anthropic + 🟡 Supabase (3 поля) = **демо з AI + realtime**. Voice (🟢) додає wow → бери раз вже йдеш по ключі.
