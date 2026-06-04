# Module: Auth Screens — реєстрація / вхід
**Дата:** 2026-06-04
**Клієнт:** [starter/STARTER.md](../../starter/STARTER.md) §2 (Supabase + SecureStore)

---

## ⚠️ Для буткемпу: auth ОПЦІЙНИЙ
Багато 4h-продуктів краще БЕЗ auth (Team Pulse — анонімний, економить годину).
Бери auth тільки якщо продукт реально потребує акаунтів (персональні дані, історія).
Альтернатива: anonymous device id (nanoid) для персоналізації без auth.

---

## Email + Password (Supabase, ~20 хв)

### `lib/auth.ts`
```typescript
import { supabase } from './supabase'

export const signUp = (email: string, password: string) =>
  supabase.auth.signUp({ email, password })

export const signIn = (email: string, password: string) =>
  supabase.auth.signInWithPassword({ email, password })

export const signOut = () => supabase.auth.signOut()
```

### `app/(auth)/login.tsx`
```tsx
import { useState } from 'react'
import { View, Text, TextInput, Pressable, Alert } from 'react-native'
import { signIn } from '@/lib/auth'
import { useRouter } from 'expo-router'

export default function Login() {
  const [email, setEmail] = useState('')
  const [pw, setPw] = useState('')
  const [loading, setLoading] = useState(false)
  const router = useRouter()

  const submit = async () => {
    setLoading(true)
    const { error } = await signIn(email, pw)
    setLoading(false)
    if (error) Alert.alert('Помилка', error.message)
    else router.replace('/(tabs)')
  }

  return (
    <View style={{ flex: 1, padding: 24, justifyContent: 'center', backgroundColor: '#fff' }}>
      <Text style={{ fontSize: 28, fontWeight: '800', marginBottom: 24 }}>Вхід</Text>
      <TextInput value={email} onChangeText={setEmail} placeholder="Email"
        autoCapitalize="none" keyboardType="email-address"
        style={{ borderWidth: 1, borderColor: '#e5e7eb', borderRadius: 12, padding: 16, marginBottom: 12 }} />
      <TextInput value={pw} onChangeText={setPw} placeholder="Пароль" secureTextEntry
        style={{ borderWidth: 1, borderColor: '#e5e7eb', borderRadius: 12, padding: 16, marginBottom: 20 }} />
      <Pressable onPress={submit} disabled={loading}
        style={{ backgroundColor: '#6366f1', borderRadius: 12, padding: 16 }}>
        <Text style={{ color: '#fff', fontWeight: '700', textAlign: 'center', fontSize: 16 }}>
          {loading ? '...' : 'Увійти'}
        </Text>
      </Pressable>
    </View>
  )
}
```

Auth-бар'єри (захищені tabs) — повний патерн у [gemini-expo-mvp-deep.md](../gemini-expo-mvp-deep.md) §4.

---

## Apple / Google Sign-In (соц, для прод)
```bash
npx expo install expo-apple-authentication
```
⚠️ Потребує dev build + налаштування в Apple/Google Console + Supabase provider.
НЕ для 4h-демо. Якщо треба "виглядати як справжній" → кнопки-заглушки соц-логіну.

## Час
- Email/password (з auth-бар'єрами): ~20-30 хв
- Соц-логін: 1+ год (dev build) — пропусти на буткемпі
