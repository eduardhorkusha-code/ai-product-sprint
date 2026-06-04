# Callstack Agent-Skills — Повний розбір
**Джерело:** Gemini Round 3 PDF + Grok competitive intelligence  
**Репо:** https://github.com/callstackincubator/agent-skills  
**Дата:** 2026-06-04

---

## Встановлення в проект

```bash
# Варіант 1 — npm plugin (рекомендований для Claude Code)
npx @callstack/react-native-ai-skills init
npx @callstack/react-native-ai-skills --add-to-claude-code

# Варіант 2 — clone напряму в .cursor/skills/ (для Cursor)
git clone https://github.com/callstackincubator/agent-skills \
  .cursor/skills/agent-skills
```

### Підключення до CLAUDE.md

Додати в CLAUDE.md секцію:
```markdown
## React Native Performance & Architecture Protocol

When reviewing, writing, or optimizing React Native code, reference and enforce:
- .claude/skills/agent-skills/skills/react-native-best-practices/SKILL.md
- .claude/skills/agent-skills/skills/react-native-best-practices/references/

Core measurement loop: Measure → Optimize → Re-measure → Validate.
```

### Також існує: expo/skills (офіційний від Expo)

```bash
# Expo-специфічні skills
npx expo install expo/skills
```
Включає: `use-dom` (DOM components guide), `building-native-ui` (routing+styling), `expo-tailwind-setup` (v4/v5 nativewind).

### Та: devanshuDesai/agent-skills

```bash
# Design review skills (Apple HIG)
git clone https://github.com/devanshudesai/agent-skills
```
Структуровані design reviews на основі Apple Human Interface Guidelines.

---

## Повний каталог файлів agent-skills

| Файл | Домен | Що робить для агента |
|------|-------|---------------------|
| `skills/react-native-best-practices/SKILL.md` | Core Reference | Маппінг bottlenecks → optimization guidelines + команди |
| `references/js-lists-flatlist-flashlist.md` | List Rendering | Замінити важкі scroll containers на FlashList |
| `references/js-animations-reanimated.md` | Animation Physics | Offload layout calculations на native UI thread через shared values |
| `references/js-measure-fps.md` | Frame Rate Auditing | Shell scripts + devtools для ізоляції FPS drops |
| `references/js-profile-react.md` | DevTools Profiling | React DevTools setup для виявлення costly re-renders |
| `references/js-memory-leaks.md` | Memory Management | JavaScript memory leaks через stale variable references |
| `references/js-bottomsheet.md` | UI Navigation | High-frequency layout animations на native threads |
| `references/js-concurrent-react.md` | Concurrent UI | Async React transitions для defer важких обчислень під анімацію |
| `references/native-memory-leaks.md` | Native Allocation | Diagnostic workflow для system memory across native platforms |
| `references/bundle-analyze-js.md` | Bundle Optimization | Automated visualizers для видалення важких пакетів |
| `references/bundle-barrel-exports.md` | Import Structuring | Eliminate barrel exports → direct path imports → менший bundle |
| `references/native-measure-tti.md` | Cold Start Auditing | TTI metrics для initial app load performance |
| `skills/react-native-brownfield-migration/SKILL.md` | Brownfield Integration | Інтеграція RN/Expo в existing native apps |
| `references/quick-start.md` | Workspace Routing | Класифікує архітектуру → Expo prebuild або bare |
| `references/expo-create-app.md` | App Scaffolding | Автоматизує створення clean Expo app context |
| `references/expo-quick-start.md` | Config Plugins | Autolinking plugins для smooth native compilation |
| `references/bare-quick-start.md` | Bare Integration | iOS CocoaPods + Android Gradle dependencies |
| `references/bare-ios-xcframework-generation.md` | iOS Compilation | Compile iOS modules у static framework archives |
| `references/bare-android-aar-generation.md` | Android Compilation | Bundle Android codebases в Android Archive libs |
| `references/bare-android-native-integration.md` | Native Runtime | Native host managers для React Native fragments |
| `skills/upgrading-react-native/SKILL.md` | Platform Migration | Safe framework upgrades across core dependencies |
| `references/upgrading-dependencies.md` | Dependency Auditing | Ідентифікація та вирішення package mismatches |
| `references/react.md` | React 19 Syncing | Align testing/runtime з new React + compiler configs |
| `references/expo-sdk-upgrade.md` | SDK Migration | Automated cache-clearing + global packages для target Expo SDK |
| `references/upgrade-helper-core.md` | Code Resolution | Community upgrade tools для native config conflicts |
| `references/upgrade-verification.md` | Visual Validation | Automated device simulation для UI verification post-upgrade |

---

## 5 Галюцинацій що agent-skills запобігає

**Критично для CLAUDE.md** — ці правила запобігають найпоширенішим помилкам моделі:

### 1. Speculative hooks optimization
❌ Агент огортає все в `useMemo`/`useCallback` "на всяк випадок"  
✅ Правило: **не додавати useMemo/useCallback без profiler evidence**

### 2. Outdated package diagnostics  
❌ Агент рекомендує deprecated layout props у FlatList  
✅ Правило: **перевіряти API compatibility перед code changes**

### 3. Layout-depth assumptions
❌ Агент вважає, що глибина component tree = причина slow renders  
✅ Правило: **покладатись на explicit timeline measurements, не припущення**

### 4. State pollution in animations
❌ Агент використовує `useState` в high-frequency UI threads (анімації)  
✅ Правило: **використовувати Reanimated shared values, не React state**

### 5. Generic barrel exports
❌ Агент пише `import { X } from '@/components'` через barrel index  
✅ Правило: **direct imports** `import X from '@/components/X'` → менший bundle

---

## Які skills цінні для 1-day hackathon

| Skill | Цінність для буткемпу | Чому |
|-------|----------------------|------|
| `js-animations-reanimated.md` | 🔴 MUST | Анімації на native thread = плавний demo |
| `js-lists-flatlist-flashlist.md` | 🔴 MUST | FlashList правильно з першого разу |
| `js-profile-react.md` | 🟡 HIGH | Якщо щось лагає під час демо |
| `bundle-barrel-exports.md` | 🟡 HIGH | Прямий вплив на compile time |
| `js-measure-fps.md` | 🟢 NICE | Тільки якщо помітні FPS drops |
| `native-memory-leaks.md` | ❌ SKIP | Не критично для 1-day demo |
| `react-native-brownfield-migration` | ❌ SKIP | Не потрібно для нового проекту |
| `upgrading-react-native` | ❌ SKIP | Не потрібно |

**Для буткемпу**: підключаємо тільки `react-native-best-practices` skill. Brownfield та upgrading — зайво.

---

## Callstack — що варто взяти для буткемпу

**Liquid Glass UI** — компонент бібліотека з advanced animations:
```bash
# Дивись: github.com/callstack (Liquid Glass repo)
# Використовуй як референс для beautiful components
```

**Re.Pack** — НЕ для буткемпу (Module Federation занадто складно для 1 дня)

**React Native Paper** — зрілий Material Design components, якщо не хочеш NativeWind:
```bash
npx expo install react-native-paper
```
