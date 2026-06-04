# ai-product-sprint

**SKELAR × Claude Bootcamp — "Creating Digital Product"**
⏱️ **Субота 6 червня 2026, 15:00–19:00 = 4 ГОДИНИ**
Mentor: @Andrii Zadorozhnyi · Команда: Eduard + AI-агенти · Ціль: за 4 години побудувати mobile productivity app і виграти голосування в #platform_ai

> Цей репо — база знань і команда агентів для перемоги. Все зібрано заздалегідь, щоб у день X не витрачати час і токени на пошук.
> 🎯 **Головний документ дня: [PLAYBOOK.md](PLAYBOOK.md)** — хвилинний план 4 годин.

---

## 🚀 Швидкий старт (нова сесія — читай це)

1. **[PLAYBOOK.md](PLAYBOOK.md)** — 🎯 хвилинний план 4 годин + мій воркфлоу
2. **[LOG.md](LOG.md)** — секція "ЗАРАЗ" показує де ми стоїмо просто зараз
3. **[research/INDEX.md](research/INDEX.md)** — навігатор бази знань за фазами спринту
4. **[research/STATUS.md](research/STATUS.md)** — що покрито (ресерч 100%, всі плями закриті)
5. **[SETUP.md](SETUP.md)** — підготовка середовища + порядок старту в день X
6. **[GSTACK.md](GSTACK.md)** — операційна система команди (фаза→скіл→хто)
7. **[starter/MODULES.md](starter/MODULES.md)** — 🧱 каталог-конструктор (обери модулі/набір → збери)
8. **[starter/STARTER.md](starter/STARTER.md)** — готовий код для копіювання в день X

На буткемпі: завдання → `research/INDEX.md` → вибрати продукт → `/office-hours` → `/spec` → build.

---

## 🧠 База знань — карта

### 🎯 Стратегія (ЧИТАТИ ПЕРШИМИ)
| Файл | Що всередині |
|------|-------------|
| [winning-formula.md](research/strategy/winning-formula.md) | **НАЙВАЖЛИВІШИЙ.** Feature Vote Conversion, Social Proof Moment, 5-step перемога |
| [organizer-energy-thesis.md](research/strategy/organizer-energy-thesis.md) | 🎯 теза організатора "Енергія = NEW GOLD" — тюнінг продукту під суддів |
| [bootcamp-day-plan.md](research/strategy/bootcamp-day-plan.md) | ⚠️ 8h таймінг застарів (див. PLAYBOOK) — але scope rules + backup strategies актуальні |
| [ux-demo-psychology.md](research/strategy/ux-demo-psychology.md) | Сторітелінг, "10× момент", demo скрипт 0:00–3:30 |
| [hackathon-trends-2026.md](research/strategy/hackathon-trends-2026.md) | Що виграє 2026, нові бібліотеки, SDK 53 сумісність |
| [claude-code-expo-setup.md](research/strategy/claude-code-expo-setup.md) | CLAUDE.md шаблон для Expo, типові помилки моделі |

### 💻 Код / Модулі (copy-paste ready)
> 🧱 Повний конструктор з наборами — [starter/MODULES.md](starter/MODULES.md). Нижче — всі 15 цеглинок.

| Файл | Що всередині |
|------|-------------|
| [starter-templates.md](research/code/starter-templates.md) | toyamarodrigo шаблон (переможець), NativeWind setup |
| [core-patterns.md](research/code/core-patterns.md) | AsyncStorage, Zustand, streak, FlashList, Haptics, Sharing |
| [supabase-schemas.md](research/code/supabase-schemas.md) | SQL + 5 RLS-патернів + SecureStore + realtime |
| [zustand-flashlist-complete.md](research/code/zustand-flashlist-complete.md) | Повний store з undo + FlashList + Top-10 Expo errors |
| [animations.md](research/code/animations.md) | 6 Reanimated анімацій (Checkbox→Streak Fire) |
| [charts.md](research/code/charts.md) | react-native-graph (Skia) / Victory / progress ring |
| [ai-features.md](research/code/ai-features.md) | 5 AI-патернів, Claude API + Vercel AI SDK streaming |
| [voice-to-text.md](research/code/voice-to-text.md) | on-device (uk-UA) або Whisper API |
| [onboarding-funnel.md](research/code/onboarding-funnel.md) | 🎯 quiz→портрет→paywall (SKELAR-бізнес) |
| [admin-dashboard.md](research/code/admin-dashboard.md) | 🎯 MRR/воронка/active (повний продукт) |
| [payment-flow.md](research/code/payment-flow.md) | Paywall→підписка (мок на демо, RevenueCat прод) |
| [auth-screens.md](research/code/auth-screens.md) | Реєстрація/вхід Supabase (опційно) |
| [app-essentials.md](research/code/app-essentials.md) | Notifications/analytics/profile/permissions |
| [in-app-feedback.md](research/code/in-app-feedback.md) | Toast/banner (фейк push) |
| [callstack-agent-skills.md](research/code/callstack-agent-skills.md) | RN performance skills, 5 галюцинацій що запобігає |

### 🎨 Дизайн
| Файл | Що всередині |
|------|-------------|
| [ui-patterns.md](research/design/ui-patterns.md) | Кольори, spacing, AI slop checklist, компоненти |

### 💡 Продукт
| Файл | Що всередині |
|------|-------------|
| [ideas-catalog.md](research/products/ideas-catalog.md) | 10 ідей, **4h-READY топ-3** (Team Energy Pulse #1) |

### 🔧 Інструменти та інфраструктура
| Файл | Що всередині |
|------|-------------|
| [tools-ecosystem.md](research/tools-ecosystem.md) | Figma, v0, Expo Orbit, Supabase MCP, pre-bootcamp checklist |
| [gemini-bootcamp-infrastructure.md](research/gemini-bootcamp-infrastructure.md) | Claude Code, MCP, worktrees, memory, карта груп-конкурентів |
| [gemini-expo-mvp-deep.md](research/gemini-expo-mvp-deep.md) | SecureStore, 'use dom', карта 8 помилок Expo |

### 📝 Промпти для ресерчу (резерв)
| Файл | Для чого |
|------|----------|
| [prompts-for-other-ais.md](research/prompts-for-other-ais.md) | ChatGPT/Grok/Perplexity — закриті ✅ |
| [prompts-ai-integration.md](research/prompts-ai-integration.md) | AI в Expo — закрито ✅ |
| [gemini-round3-prompt.md](research/gemini-round3-prompt.md) | Callstack/templates — закрито ✅ |

---

## 🤖 AI-команда

| Агент | Codename | Файл | Роль | Ключовий скіл |
|-------|----------|------|------|--------------|
| **Product Brain** | Nick Fury | [agents/product-brain.md](agents/product-brain.md) | PM + Research | `/office-hours`, `/spec` |
| **Builder** | Tony Stark | [agents/builder.md](agents/builder.md) | Dev (mobile-first) | `/autoplan`, `/investigate` |
| **Design Eye** | Da Vinci | [agents/design-eye.md](agents/design-eye.md) | UX/iOS дизайн | `/design-review` |
| **AI Engineer** | Vision | [agents/ai-engineer.md](agents/ai-engineer.md) | AI-фічі (Claude, voice→text, realtime) | `/investigate` |
| **Demo Director** | Mysterio | [agents/demo-director.md](agents/demo-director.md) | Pitch + демо + конверсія голосів | `/document-release` |
| **QA Tester** | Spider-Man | [agents/qa-tester.md](agents/qa-tester.md) | Щоб не впало на демо | `/qa`, `/canary` |
| **Eduard** | — | — | Sprint Clock + рішення | `/autoplan` щоб різати scope |

**Flow:** Product Brain (scope) → Builder + AI Engineer (build) → Design Eye (polish) → QA Tester (Go/No-Go) → Demo Director (pitch + vote)

---

## ⚙️ Стек (зафіксовано)

```
Expo SDK 53 + Expo Router v3 (typedRoutes)
  + NativeWind v4.2.1  (НЕ 4.2.0 — зламана)
  + Reanimated + Gesture Handler
  + Zustand + AsyncStorage
  + Supabase (auth через expo-secure-store, НЕ AsyncStorage!)
  + Claude API / Vercel AI SDK (для AI-фічі)

Шаблон: toyamarodrigo/expo-router-template (FREE MIT, 5хв setup)
```

---

## 🏆 Winning Formula (TL;DR)

```
Instant value (секунди) + Live realtime + Social Proof Moment = WIN
```

1. **Instant Onboarding** (vote power 9.8) — нуль setup screens, value одразу
2. **Live Realtime** (9.2) — Supabase realtime, екран оновлюється під час демо
3. **Social Proof Moment** — "Шліть emoji в #platform_ai, дивимось як екран оживає live"
4. **AI that ACTS** — voice→action виграє над "AI that summarizes"
5. **10× момент** у демо-відео — крупно, з контрастом, "+100% ефективності"

---

## Принципи спринту (gstack ETHOS)

- **Boil the Lake** — повна фіча, не 80%. З AI 20% коштує секунди.
- **Search Before Building** — перевір що є, перш ніж писати з нуля.
- **Demo > Perfect** — робочий UI б'є чисту архітектуру.
- **Cut > Debate** — сумніваєшся → ріж фічу.

Походить з SKELAR reputation-dashboard досвіду: [reputation-dashboard](https://github.com/eduardhorkusha-code/reputation-dashboard).
