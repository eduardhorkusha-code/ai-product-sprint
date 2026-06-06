# Vercel AI Proxy — готовий до деплою

Мінімальний Vercel-проєкт що ховає `ANTHROPIC_API_KEY` і дає застосунку безпечний endpoint. Standup ~10 хв.

```
Expo App ──fetch──→ Vercel /api/claude ──SDK──→ Claude API (ключ тут)
```

---

## Крок 1 — Створи проєкт-папку (окремо від застосунку)

```bash
mkdir ai-proxy && cd ai-proxy
```

### `package.json`
```json
{
  "name": "ai-product-sprint-proxy",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "@anthropic-ai/sdk": "latest"
  }
}
```

### `api/claude.ts` — основний endpoint (надійний JSON, не стрім)
```typescript
import Anthropic from '@anthropic-ai/sdk'

const anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY })

export default async function handler(req: any, res: any) {
  // CORS — щоб Expo міг звертатись
  res.setHeader('Access-Control-Allow-Origin', '*')
  res.setHeader('Access-Control-Allow-Methods', 'POST, OPTIONS')
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type')
  if (req.method === 'OPTIONS') return res.status(200).end()
  if (req.method !== 'POST') return res.status(405).json({ error: 'POST only' })

  try {
    const { prompt, system, model, json } = req.body ?? {}
    if (!prompt) return res.status(400).json({ error: 'prompt required' })

    const message = await anthropic.messages.create({
      model: model || process.env.CLAUDE_MODEL || 'claude-opus-4-8',
      max_tokens: 1024,
      ...(system ? { system } : {}),
      messages: [{ role: 'user', content: prompt }],
    })

    const text = message.content.find((b: any) => b.type === 'text')?.text ?? ''
    // json:true → намагаємось розпарсити (для task-parsing фічі)
    if (json) {
      try { return res.status(200).json({ data: JSON.parse(text), raw: text }) }
      catch { return res.status(200).json({ data: null, raw: text }) }
    }
    res.status(200).json({ text })
  } catch (e: any) {
    res.status(500).json({ error: String(e?.message ?? e) })
  }
}
```

---

## Крок 2 — Деплой

```bash
npm i -g vercel          # якщо нема
vercel                   # перший раз: створює проєкт, логін
vercel env add ANTHROPIC_API_KEY    # встав sk-ant-... (або через UI)
vercel env add CLAUDE_MODEL         # claude-opus-4-8
vercel --prod            # → отримаєш https://ai-proxy-xxx.vercel.app
```

Цей URL → у застосунок як `EXPO_PUBLIC_API_URL`.

**Перевірка що працює:**
```bash
curl -X POST https://твій-проєкт.vercel.app/api/claude \
  -H "Content-Type: application/json" \
  -d '{"prompt":"Скажи привіт одним словом"}'
# → {"text":"Привіт"}
```

---

## Крок 3 — Клієнт у застосунку

### `lib/claude.ts`
```typescript
const API_URL = process.env.EXPO_PUBLIC_API_URL!

type Opts = { system?: string; model?: string }

export async function askClaude(prompt: string, opts: Opts = {}): Promise<string> {
  const res = await fetch(`${API_URL}/api/claude`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ prompt, ...opts }),
  })
  if (!res.ok) throw new Error(`Claude proxy ${res.status}`)
  return (await res.json()).text
}

// Для фіч що повертають структуру (task parsing) — json:true
export async function askClaudeJSON<T>(prompt: string, opts: Opts = {}): Promise<T | null> {
  const res = await fetch(`${API_URL}/api/claude`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ prompt, json: true, ...opts }),
  })
  if (!res.ok) throw new Error(`Claude proxy ${res.status}`)
  return (await res.json()).data
}
```

---

## Крок 4 — Фічі що ДІЮТЬ (vote winners)

### Voice/text → структурована задача
```typescript
import { askClaudeJSON } from '@/lib/claude'

type ParsedTask = { title: string; dueDate: string | null; priority: 'high'|'medium'|'low'; tags: string[] }

export const parseTask = (input: string) =>
  askClaudeJSON<ParsedTask>(input, {
    model: 'claude-haiku-4-5',  // швидко — користувач чекає
    system: `Витягни задачу з тексту. Сьогодні ${new Date().toISOString().split('T')[0]}.
Поверни ТІЛЬКИ JSON: {"title":"дієслово+об'єкт","dueDate":"YYYY-MM-DD або null","priority":"high|medium|low","tags":[]}`,
  })
// "Зателефонувати Анні до п'ятниці" → {title:"Зателефонувати Анні", dueDate:"2026-06-12", priority:"medium", tags:["дзвінок"]}
```

### "Що робити зараз?" — AI-пріоритет
```typescript
export const suggestNext = (tasks: {title:string; priority:string}[]) =>
  askClaude(`Задачі: ${JSON.stringify(tasks)}. Що робити ПРЯМО ЗАРАЗ і чому? 1 речення.`, {
    model: 'claude-opus-4-8',  // якість важливіша за швидкість тут
  })
```

### Дайджест дня
```typescript
export const dailyDigest = (done: string[], pending: string[]) =>
  askClaude(`Зроблено: ${done.join(', ')}. Лишилось: ${pending.join(', ')}.
Дай теплий підсумок дня (2 речення) + 1 фокус на завтра.`)
```

---

## Вибір моделі (важливо для демо)

| Фіча | Модель | Чому |
|------|--------|------|
| Парсинг задач, категоризація | `claude-haiku-4-5` | Користувач чекає → ~1с відгук = жива демка |
| "Що робити", дайджест, інсайти | `claude-opus-4-8` | Якість міркування = wow-момент |

На live-демо лаг у 5с вбиває енергію. Швидкі фічі → Haiku. Глибокі → Opus.

---

## Якщо хочеш стрім (chat UI)
Vercel AI SDK `useChat` + Anthropic provider — див. research/code/ai-features.md § "Vercel AI SDK useChat". Для парсингу/дайджесту стрім не потрібен — JSON надійніший.
