# Agent Files

Кожен `.md` — визначення Claude Code sub-agent. Стиль і структура наслідують
[ai-team](https://github.com/eduardhorkusha-code/ai-team): YAML frontmatter (model, tools,
codename), Voice (характер супергероя), Experience Log (вчаться на своїх уроках).

## Installation

```bash
# Проектні — тільки для цього репо
cp agents/*.md .claude/agents/

# Або глобальні — у всіх проектах
cp agents/*.md ~/.claude/agents/
```

Experience logs живуть у `.claude/agents/memory/` (version-controlled, seed-уроки вже є).

## Команда

| Файл | Агент | Codename | Модель | Чому ця модель |
|------|-------|----------|--------|----------------|
| `product-brain.md` | 🟣 Product Brain (PM) | Nick Fury | **Opus** | Scope-рішення = найвища judgment-робота |
| `builder.md` | 🔵 Builder (Dev) | Tony Stark | Sonnet | Швидке виконання за патернами |
| `design-eye.md` | 🩷 Design Eye (UX) | Da Vinci | Sonnet | Аудит за чеклістом, методично |
| `ai-engineer.md` | 🟣 AI Engineer | Vision | Sonnet | Код + швидкість (ескалює до Opus на hard prompts) |
| `demo-director.md` | 🟢 Demo Director | Mysterio | **Opus** | Сторітелінг + психологія голосів = judgment |
| `qa-tester.md` | 🟡 QA Tester | Spider-Man | Sonnet | Систематична перевірка станів |

6 агентів + Eduard (Sprint Clock). Модель кожен обрав під свою роль: Opus там де
рішення вирішують день (scope, demo), Sonnet там де темп і виконання.

## Flow

```
Product Brain (scope) → Builder + AI Engineer (build) → Design Eye (polish)
  → QA Tester (Go/No-Go) → Demo Director (pitch + vote)
```

## Як агенти вчаться (Experience Log)

Кожен агент на ПОЧАТКУ задачі читає `.claude/agents/memory/<name>-experience.md`,
у КІНЦІ дописує урок. Так знання накопичується між сесіями — наступний раз агент
не наступає на ті ж граблі. Seed-уроки з research вже закладені.

Систематизація роботи — через gstack: див. [GSTACK.md](../GSTACK.md).
