# AI Features in Expo — Ready Patterns
**Джерело:** Grok report (AI integration prompt)  
**Дата:** 2026-06-04

---

## 🥇 Рекомендація #1: AI Meeting Action Item Extractor

> "AI що слухає твою voice note і миттєво витягує action items, owners, deadlines"

- **Expo/RN API:** `expo-audio` (recording) + `expo-file-system`
- **AI Provider:** Claude Haiku (via Vercel AI SDK або direct)
- **Build time:** 35-45 хв
- **Wow moment:** Записуєш 20-сек voice note → AI видає список задач з дедлайнами. Корпоративні судді: "це реально економить мені 2 години на тиждень".

---

## 5 AI-патернів за швидкістю

| # | Фіча | Expo API | Build | Wow для non-tech |
|---|------|----------|-------|-----------------|
| ⭐ | **Meeting Action Extractor** (voice→tasks) | expo-audio | 35-45хв | Voice → структуровані задачі з дедлайнами |
| 1 | **AI Smart Summarizer** (paste→bullets) | Clipboard + TextInput | 20-25хв | "Довгий meeting note → 3-бульветний summary" |
| 2 | **AI Email Drafter** (voice→email) | expo-audio → text | 30-40хв | "Напиши начальнику що запізнюсь" → формальний email |
| 3 | **AI Task Prioritizer** | Text list input | 25-30хв | 10 задач → AI ранжує + часові блоки |
| 4 | **AI Tone Adapter** | Text input | 25хв | Грубий текст → Professional/Friendly/Assertive версії |

**Правило:** фічі buildable за 30-60 хв. Не RAG, не multi-agent.

---

## Claude API direct call з Expo (без backend)

### Safety
- ⚠️ API key буде в bundle → легко витягти (навіть в Expo Go)
- **Hackathon:** OK (direct call, 30 сек setup)
- **Production:** НІКОЛИ — будь-хто декомпілює APK/IPA → отримає ключ → ти платиш за чужі запити

### Архітектура
- **Hackathon (speed):** Direct call
- **Production:** Expo API Routes (Route Handlers) → proxy до Anthropic

### Working code — direct streaming

```bash
npm install ai @ai-sdk/anthropic
```

```tsx
// utils/claude.ts
import { createAnthropic } from '@ai-sdk/anthropic'
import { streamText } from 'ai'

const anthropic = createAnthropic({
  apiKey: process.env.EXPO_PUBLIC_ANTHROPIC_API_KEY!,
})

export async function* generateWithClaude(prompt: string) {
  const result = streamText({
    model: anthropic('claude-haiku-4-5'), // fastest + cheap
    prompt,
    temperature: 0.7,
  })

  for await (const chunk of result.textStream) {
    yield chunk
  }
}
```

```tsx
// Використання в компоненті
const [response, setResponse] = useState('')
const [isStreaming, setIsStreaming] = useState(false)

const runAI = async () => {
  setIsStreaming(true)
  setResponse('')

  for await (const chunk of generateWithClaude("Extract action items from: " + userText)) {
    setResponse(prev => prev + chunk)
  }

  setIsStreaming(false)
}
```

---

## Vercel AI SDK useChat (streaming chat UI)

```tsx
// app/ai-stream.tsx
import { useState } from 'react'
import { View, Text, TextInput, Button, ScrollView } from 'react-native'
import { useChat } from 'ai/react'

export default function AIStream() {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat({
    api: '/api/chat', // Expo API Route (рекомендовано для production)
    initialMessages: [],
  })

  return (
    <ScrollView className="flex-1 p-4 bg-gray-50">
      {messages.map(m => (
        <View key={m.id} className={`mb-4 ${m.role === 'user' ? 'items-end' : 'items-start'}`}>
          <Text className={`px-4 py-3 rounded-2xl max-w-[80%] ${m.role === 'user' ? 'bg-blue-600 text-white' : 'bg-white'}`}>
            {m.content}
          </Text>
        </View>
      ))}
      <TextInput
        value={input}
        onChangeText={handleInputChange}
        placeholder="Що потрібно зробити?"
        className="border border-gray-300 rounded-xl p-4 mt-4"
      />
      <Button
        title={isLoading ? "AI думає..." : "Надіслати"}
        onPress={handleSubmit}
        disabled={isLoading}
      />
    </ScrollView>
  )
}
```

---

## AI фічі за corporate vote conversion (Grok ranking)

**Voting winners (HR/Finance/PM/Legal):**
1. **Action Items + Task Extraction** з meeting notes/voice (топ-1)
2. **Smart Email/Slack Drafter** з tone control
3. **Meeting Summary + Key Decisions**
4. **Expense/Receipt Categorizer** (Finance)
5. **Policy/Compliance Checker** (Legal)

**Impressive але low actual usage (НЕ роби):**
- AI Image generators
- Fancy chatbots без actions
- Sentiment analysis без рекомендацій

**Ключова різниця в реакції:**
- "AI that summarizes text" → ввічливі оплески ("корисно")
- **"AI that takes an action based on what you say"** → standing ovation + "можна це в продакшн?!"

→ **Будуй фічі що ДІЮТЬ, не просто аналізують.**
