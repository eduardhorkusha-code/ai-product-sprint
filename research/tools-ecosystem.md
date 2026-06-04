# Інструменти для розробки — що підключати
**Оновлено:** 2026-06-04  
**Мета:** максимально підсилити швидкість розробки під час буткемпу

---

## TL;DR — що підключити прямо зараз

| Пріоритет | Інструмент | Час налаштування | Вплив на швидкість |
|-----------|-----------|-----------------|-------------------|
| 🔴 MUST | **Expo Go на телефоні** | 2 хв | Real device testing без кабелю |
| 🔴 MUST | **Expo Orbit (Mac)** | 5 хв | Автооновлення simulator одним кліком |
| 🟡 HIGH | **v0 by Vercel** | 0 хв | UI generation → React Native адаптація |
| 🟡 HIGH | **Figma (Community)** | 10 хв | Готові mobile UI kits для референсу |
| 🟢 NICE | **Supabase MCP** | 15 хв | Claude пише SQL напряму у твою БД |
| 🟢 NICE | **Expo Snack** | 0 хв | Швидкий прототип без setup |

---

## Figma — так, варто підключити

### Навіщо Figma на буткемпі

**НЕ для** детального дизайну (немає часу).  
**ДЛЯ:**
1. Взяти готовий mobile UI kit → скопіювати layout → задати Claude "реалізуй цей екран"
2. Показати дизайн менторам ДО кодування (замість слів)
3. Community Files — безкоштовні Figma компоненти для iOS

### Figma Community Files (безкоштовно, скопіюй зараз)

| Файл | Що всередині | Посилання |
|------|-------------|-----------|
| **iOS 17 UI Kit** | Нативні iOS компоненти, точний spacing | figma.com/community → search "iOS 17 UI Kit" |
| **Mobile App UI Kit** | 100+ мобільних екранів | figma.com/community → "Mobile App UI Kit" |
| **Habit Tracker App** | Готові екрани habit tracker | figma.com/community → "Habit Tracker" |
| **Productivity App UI** | Task/todo screen layouts | figma.com/community → "Productivity App" |

### Figma → Claude Code workflow

```
1. Відкрий Community File
2. Знайди потрібний екран
3. Зроби screenshot або Export as PNG
4. Завантаж в Claude: "Реалізуй цей екран в React Native + NativeWind"
5. Claude генерує компонент за скриншотом
```

### Figma MCP (опційно, але потужно)

```bash
# Встанови Figma MCP для Claude Code
# Дозволяє Claude читати твій Figma файл напряму

# В Settings → Connectors в Claude Desktop додай:
# Figma Dev Mode MCP Server
```

**Що дає:** Claude бачить твій Figma файл, читає компоненти, 
генерує код з точними розмірами і кольорами.  
**Налаштування:** 15 хвилин, потрібен Figma Professional або Figma Dev Mode.

---

## v0 by Vercel — найшвидший UI generator

**URL:** v0.dev  
**Що робить:** генерує React UI за текстовим описом

### Workflow для React Native

```
1. Опиши екран на v0.dev:
   "A habit tracker screen with a list of habits, each showing 
   name, emoji, streak count, and a check button. 
   Clean minimal design, indigo primary color."

2. v0 генерує React/Tailwind код

3. Кажеш Claude: "Адаптуй цей v0 компонент для React Native + NativeWind.
   Заміни div → View, p → Text, className → className NativeWind"

4. За 10 хвилин маєш готовий екран
```

**Обмеження:** v0 генерує web React, не RN — але адаптація 70% готового коду краща ніж 0%.

---

## Expo Orbit — MUST HAVE на Mac

**URL:** expo.dev/orbit  
**Що робить:** menu bar app, запускає builds на simulator в 1 клік

```bash
# Install
brew install expo-orbit
# або скачай .dmg з expo.dev/orbit
```

**Без Orbit:** `npx expo start` → відкрити браузер → знайти QR → чекати  
**З Orbit:** один клік у menu bar → simulator відкрився з оновленням

---

## Supabase MCP — Claude пише SQL напряму

```bash
# В .claude/settings.json або Claude Desktop → Settings → Developer → MCP
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server-supabase@latest",
               "--supabase-access-token", "YOUR_SUPABASE_PAT"]
    }
  }
}
```

**Що дає під час буткемпу:**
- Кажеш Claude: "Створи таблицю habits з RLS для anonymous users"
- Claude сам виконує SQL в твоїй БД через MCP
- Не треба відкривати Supabase Dashboard

**Де взяти PAT:** supabase.com → Account → Access Tokens

---

## Bolt.new — альтернатива v0 для повних апок

**URL:** bolt.new  
**Що робить:** генерує повний проект (React / Next.js) за промптом

### Для буткемпу — не для основного коду, але для:
1. Швидко перевірити чи ідея реалізується → "згенеруй habit tracker"
2. Взяти конкретний компонент → адаптувати в RN
3. Показати дизайн-концепт менторам до початку кодування

---

## Lovable.dev — UI прототипи

**URL:** lovable.dev  
**Що робить:** генерує повний React + Supabase проект  
**Для буткемпу:** швидкий прототип для валідації ідеї (не production код)

---

## Expo Snack — швидкі прототипи без setup

**URL:** snack.expo.dev  
**Що робить:** Expo в браузері, без установки  
**Коли:** перевірити анімацію / компонент без запуску всього проекту

---

## Screen Recording для Demo

### Mac (безкоштовно)
```
QuickTime → File → New Screen Recording → вибери iPhone Mirroring або iPhone через USB
```

### iPhone Mirroring (macOS Sequoia+)
- Дзеркалить iPhone екран на Mac
- Record через QuickTime
- Немає затримки
- Виглядає як нативний демо

### Альтернатива — Rotato
**URL:** rotato.app  
**Що робить:** записує screen + додає красивий iPhone frame автоматично  
**Результат:** professional demo video за 5 хвилин

---

## Git Flow для буткемпу (спрощений)

```bash
# Ранок: один раз
git clone <repo> && cd <project>

# Під час розробки: commit кожні 30-45 хвилин
git add -A && git commit -m "feat: habit check animation"

# Якщо щось зламав: швидкий rollback
git stash           # сховати незбережене
git log --oneline   # знайти останній робочий коміт
git checkout <hash> -- app/   # відновити тільки app/ папку

# НЕ витрачай час на: feature branches, PRs, code review під час буткемпу
```

---

## Pre-bootcamp Setup Checklist

```
За тиждень до буткемпу:
[ ] Expo Go встановлено на iPhone (App Store)
[ ] Expo Orbit встановлено на Mac (expo.dev/orbit)
[ ] Node.js 20+ встановлено: node --version
[ ] Bun встановлено: bun --version (або npm fallback)
[ ] Claude Code CLI: claude --version
[ ] Test: clone toyamarodrigo template → bun install → expo start → відкрити в Expo Go ✅

[ ] Figma аккаунт є (безкоштовний)
[ ] Збережено 2-3 Community mobile UI kits
[ ] v0.dev відкрито і перевірено

[ ] Supabase проект створено: app.supabase.com → New Project
[ ] .env.local файл з EXPO_PUBLIC_SUPABASE_URL + EXPO_PUBLIC_SUPABASE_ANON_KEY

[ ] Seed data підготована (research/strategy/claude-code-expo-setup.md)
[ ] research/INDEX.md відкрита та прочитана
```
