# Voice → Text транскрипція (закриває темну пляму #1)
**Джерело:** WebSearch (jamsch/expo-speech-recognition, Whisper API, LogRocket)
**Дата:** 2026-06-04

---

## TL;DR — два шляхи

| Шлях | Backend? | Точність | Швидкість | Коли |
|------|----------|----------|-----------|------|
| **expo-speech-recognition** (on-device) ⭐ | ❌ ні | добра | миттєва | Буткемп — найшвидший |
| **Whisper API** (OpenAI) | ⚠️ ключ | відмінна | ~1-2с | Якщо треба максимальна точність |

**Рекомендація для буткемпу:** `@jamsch/expo-speech-recognition` — on-device, нуль backend,
працює в реальному часі. Whisper як fallback якщо точність критична.

---

## Шлях 1: expo-speech-recognition (on-device) ⭐

```bash
npx expo install @jamsch/expo-speech-recognition
```

> ⚠️ Потребує **dev build** (не чистий Expo Go), бо це нативний модуль.
> `npx expo run:ios` один раз. Якщо строго Expo Go → шлях 2 (Whisper).

```tsx
// lib/useVoiceToText.ts
import { useState } from 'react'
import {
  ExpoSpeechRecognitionModule,
  useSpeechRecognitionEvent,
} from '@jamsch/expo-speech-recognition'

export function useVoiceToText() {
  const [transcript, setTranscript] = useState('')
  const [recording, setRecording] = useState(false)

  useSpeechRecognitionEvent('result', (event) => {
    setTranscript(event.results[0]?.transcript ?? '')
  })
  useSpeechRecognitionEvent('end', () => setRecording(false))

  const start = async () => {
    const perm = await ExpoSpeechRecognitionModule.requestPermissionsAsync()
    if (!perm.granted) return
    setTranscript('')
    setRecording(true)
    ExpoSpeechRecognitionModule.start({
      lang: 'uk-UA',          // українська! або 'en-US'
      interimResults: true,   // показувати текст у реальному часі
      continuous: false,
    })
  }

  const stop = () => ExpoSpeechRecognitionModule.stop()

  return { transcript, recording, start, stop }
}
```

```tsx
// Використання — voice note → текст → Claude
const { transcript, recording, start, stop } = useVoiceToText()

<Pressable onPress={recording ? stop : start}>
  <Text>{recording ? '⏹ Стоп' : '🎤 Записати'}</Text>
</Pressable>
<Text>{transcript}</Text>

// Коли готово → у AI Engineer's generateWithClaude(transcript)
```

---

## Шлях 2: Whisper API (OpenAI, працює в Expo Go)

```bash
npx expo install expo-audio
```

```tsx
// lib/whisper.ts
import { useAudioRecorder, RecordingPresets } from 'expo-audio'

export async function transcribeWithWhisper(audioUri: string): Promise<string> {
  const formData = new FormData()
  formData.append('file', {
    uri: audioUri,
    type: 'audio/m4a',
    name: 'recording.m4a',
  } as any)
  formData.append('model', 'whisper-1')
  formData.append('language', 'uk') // або 'en'

  const res = await fetch('https://api.openai.com/v1/audio/transcriptions', {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${process.env.EXPO_PUBLIC_OPENAI_API_KEY}`,
    },
    body: formData,
  })
  const data = await res.json()
  return data.text
}
```

```tsx
// Запис через expo-audio → URI → Whisper
const recorder = useAudioRecorder(RecordingPresets.HIGH_QUALITY)

const startRec = () => recorder.record()
const stopRec = async () => {
  await recorder.stop()
  const text = await transcribeWithWhisper(recorder.uri!)
  // text → generateWithClaude(text)
}
```

⚠️ **Security:** `EXPO_PUBLIC_OPENAI_API_KEY` у bundle — OK для hackathon, НЕ для prod
(той самий принцип що з Claude key — research/code/ai-features.md).

---

## Повний flow: Meeting Action Extractor

```
🎤 Запис голосу (expo-speech-recognition АБО expo-audio)
  → текст (on-device або Whisper)
    → Claude Haiku: "Extract action items as JSON: [{task, owner, deadline}]"
      → парс JSON (try/catch + fallback)
        → красивий список з чекбоксами
```

Код Claude-частини — research/code/ai-features.md (streaming) + AI Engineer agent.

---

## Рішення для буткемпу
- **Якщо встигаємо dev build:** expo-speech-recognition (on-device, uk-UA, миттєво, нуль backend)
- **Якщо строго Expo Go:** expo-audio + Whisper API (потрібен OPENAI key)
- Українська мова працює в обох (`lang: 'uk-UA'` / `language: 'uk'`)
