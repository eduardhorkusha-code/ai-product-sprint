# SETUP — підготовка до буткемпу

⏱️ **Субота 6 червня, 15:00–19:00 = 4 ГОДИНИ.** Все нижче зроби ДО 15:00.

Чекліст середовища. Що зроблено автоматично — позначено ✅. Що робиш ти — позначено 👉.

---

## ⚠️ КРИТИЧНЕ: Node версія

На машині **Node v25.8.0** — занадто новий, Expo SDK 53 офіційно хоче **Node 20 LTS**.
Node 25 (непарний, bleeding-edge) може ламати Metro/install.

👉 **Перед буткемпом перемкнись на Node 20:**
```bash
# якщо є nvm
nvm install 20 && nvm use 20 && nvm alias default 20
node --version   # має бути v20.x

# якщо нема nvm
brew install nvm   # потім додай у ~/.zshrc per brew інструкцією
```

---

## Toolchain — статус

| Інструмент | Версія | Статус |
|-----------|--------|--------|
| Node | v25.8.0 | ⚠️ перемкни на 20 LTS |
| Bun | 1.3.14 | ✅ |
| npm | 11.12.1 | ✅ |
| Claude Code CLI | 2.1.101 | ✅ |
| Expo CLI | 56.1.13 | ✅ |
| Supabase CLI | 2.75.0 | ✅ (можна оновити) |
| Watchman | 2026.06.01 | ✅ встановлено цією сесією |
| Homebrew | 5.1.15 | ✅ |
| git | 2.50.1 | ✅ |

---

## 👉 Дії від тебе (потребують телефон / акаунти)

### 1. Expo Go на iPhone
App Store → "Expo Go" → встанови. Увійди своїм Expo акаунтом.

### 2. Expo Orbit на Mac (опційно, але зручно)
```bash
brew install --cask expo-orbit
```
Запускає simulator в 1 клік з menu bar.

### 3. Supabase проект — РОЗГОРНИ БЕКАП-БАЗУ ЗАВТРА (prep)
1. app.supabase.com → New Project (запам'ятай DB password)
2. Project Settings → API → скопіюй `URL` і `anon key` → у `.env.local`
   (шаблон: [starter/.env.example](starter/.env.example))
3. **SQL Editor → встав схему з [starter/BACKUP-team-energy-pulse.md](starter/BACKUP-team-energy-pulse.md) § Schema → Run.**
   Це наш найімовірніший продукт (Team Energy Pulse) + seed-дані. БД стоятиме готова —
   у суботу нуль витрат часу. Якщо тема інша — схема скидається за 2 хв.
4. Перевір realtime: Database → Replication → `pulse_votes` у `supabase_realtime`.
5. (Альтернатива) універсальна схема habits/moods/wins — у [starter/STARTER.md](starter/STARTER.md) §3.

### 4. API ключі
- **Claude:** console.anthropic.com → API Keys → новий ключ (для AI-фічі)
- **OpenAI** (опційно): platform.openai.com → тільки якщо voice→text через Whisper

### 5. Figma — готово ✅
3 UI kits у Drafts (iOS 26, iOS 18, iOS 16 Joey Banks).

### 6. 👉 Уточни формат демо — ВИРІШЕНО
Live pitch відео 5 хв, аудиторія дивиться + може сама поклацати.
**Наслідок:** застосунок має бути доступним для кліку → готуй розповсюдження:
- **Expo Go QR** — повний нативний досвід (haptics, realtime). Аудиторії треба Expo Go.
- **Web-лінк** (`bunx expo start --web` → deploy) — нуль інсталу, клік з браузера.
  Native-фічі (haptics) деградують, але realtime/UI працюють.
- Рекомендація: QR для відео-демо (повний досвід) + web-лінк у Slack для "поклацай сам".

---

## ✅ Зроблено цією сесією (автоматично)

- ✅ Watchman встановлено (Metro file watching)
- ✅ Callstack RN best-practices skill → `reference/callstack-rn-best-practices/` (31 md)
- ✅ Starter kit → `starter/`: .env.example, STARTER.md (supabase client, schema, app CLAUDE.md, metro fix)
- ✅ Команда 6 агентів + experience logs + GSTACK.md

---

## 👉 У день буткемпу — порядок старту

```bash
# 0. Node 20
nvm use 20

# 1. Клон шаблону-переможця
bunx create-expo-app MyApp --template https://github.com/toyamarodrigo/expo-router-template
cd MyApp

# 2. Скопіюй starter-артефакти (зі starter/STARTER.md):
#    - lib/supabase.ts
#    - metro.config.js фікс
#    - CLAUDE.md застосунку
#    - .env.local з ключами

# 3. Додай Callstack skills у проект
npx @callstack/react-native-ai-skills init
npx @callstack/react-native-ai-skills --add-to-claude-code

# 4. Залежності під обрану фічу
npx expo install nativewind@4.2.1 react-native-reanimated react-native-gesture-handler \
  @shopify/flash-list expo-haptics @react-native-async-storage/async-storage zustand nanoid

# 5. Запуск
bunx expo start   # сканіруй QR в Expo Go

# 6. Встав Supabase schema у SQL Editor (starter/STARTER.md §3)
```

---

## Порядок роботи в день X

1. Завдання → читай `research/INDEX.md` (decision tree)
2. Product Brain (Nick Fury): `/office-hours` → вибір продукту → `/spec`
3. Builder + AI Engineer: build за патернами з `research/code/`
4. Design Eye: `/design-review` (лови AI slop)
5. QA Tester: Go/No-Go
6. Demo Director: 5-хв pitch відео + Slack post + Social Proof Moment

Кожен агент читає свій `.claude/agents/memory/*-experience.md` на старті.
Систематизація — `GSTACK.md`.
