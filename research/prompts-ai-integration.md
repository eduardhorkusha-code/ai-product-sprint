# Промпт: AI Integration у Expo — швидкий старт
**Для:** ChatGPT o3 або Perplexity  
**Пріоритет:** HIGH — AI wins hackathons in 2026

---

## Промпт

```
I'm building a mobile productivity app with Expo SDK 53 + React Native + NativeWind v4 
for a 1-day corporate hackathon on June 7, 2026. I have 30-60 minutes to add one 
meaningful AI feature that impresses non-technical corporate employees.

Give me 5 specific AI feature patterns for Expo mobile apps:

QUESTION 1: What's the fastest AI feature to add that genuinely impresses judges?

For each pattern provide:
- The feature in 1 sentence ("AI that does X")
- Which Expo/RN API it uses
- Which AI provider/endpoint (Claude, OpenAI, Vercel AI SDK)
- Approximate build time
- The "wow moment" for a non-technical demo audience
- Code snippet (Expo + TypeScript, production quality)

Focus on features buildable in 30-60 minutes, not RAG or multi-agent systems.

---

QUESTION 2: Vercel AI SDK in Expo — minimal working setup

Show me the absolute minimal setup for:
a) Streaming text generation in a React Native Text component
b) Voice-to-AI-response (Expo Audio → transcript → AI → display)

Requirements:
- Works in Expo Go (no dev build)
- TypeScript strict
- Uses Claude claude-haiku-4-5 (cheapest, fastest)
- Complete runnable code, no placeholders

---

QUESTION 3: What AI features win corporate audience votes?

Based on your knowledge of corporate productivity tools and internal hackathons:
- Which AI features do HR/Finance/PM/Legal employees find immediately useful?
- Which AI features look impressive in demos but employees would never actually use?
- What's the difference in audience reaction between "AI that summarizes text" 
  vs "AI that takes an action based on what you say"?

Give me a ranked list of AI feature types by corporate vote conversion power.

---

QUESTION 4: Claude API direct call from Expo (no backend)

Show me how to call Claude API directly from React Native (Expo) without a backend:
- Is it safe to put ANTHROPIC_API_KEY in Expo env vars?
- What are the security implications?
- What's the recommended architecture for a hackathon (speed) vs production (security)?
- Complete working code for a simple text generation call with streaming
```
