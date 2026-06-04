# gstack — операційна система команди

> gstack робить команду з 1 людини такою, що шипить як команда з 20.
> Кожен агент читає цей файл, щоб знати ЯКИЙ скіл під ЯКУ фазу. Без імпровізації в процесі.

**Референс:** https://github.com/garrytan/gstack · локально: `~/.claude/skills/gstack/`

---

## Принцип: процес = скіли, не імпровізація

Кожна фаза роботи має свій gstack-скіл. Якщо запит підходить під скіл — викликай його
через Skill tool. Сумніваєшся — викликай скіл.

---

## Карта: фаза → скіл → хто запускає

| Фаза | Скіл | Хто | Коли |
|------|------|-----|------|
| Нова ідея, неясний scope | `/office-hours` | Product Brain | Перед першим рядком коду |
| Потрібен spec + критерії | `/spec` | Product Brain | Будь-яка фіча |
| Архітектурне рішення | `/autoplan` | Product Brain | Зачіпає >5 файлів або core |
| Дизайн ДО коду | `/plan-design-review` | Design Eye | Будь-яка UI-фіча |
| Аудит живого UI | `/design-review` | Design Eye | Після білду — лови AI slop |
| Якість коду | `/health`, `/simplify` | Builder | Рефактор / >3 файли |
| Баг, краш, дивна поведінка | `/investigate` | Builder, AI Engineer, QA | ПЕРЕД будь-яким фіксом |
| Перевірка перед демо | `/qa`, `/canary` | QA Tester | Перед презентацією |
| Demo story + реліз | `/document-release` | Demo Director | Готуємо pitch |
| Безпека (якщо API routes) | `/cso` | будь-хто | Перед деплоєм нового route |
| Зберегти контекст | `/context-save` | будь-хто | Кінець сесії |
| Відновити контекст | `/context-restore` | будь-хто | Початок сесії |

---

## Sprint Flow (буткемп, 1 день)

```
/office-hours   → Product Brain: 6 forcing questions, MVP = 1 core feature
  → /spec       → Product Brain: user story, cut list, demo script
    → build     → Builder + AI Engineer (timebox строго, /investigate на блоках)
      → /design-review → Design Eye: лови AI slop
        → /qa   → QA Tester: Go/No-Go вердикт
          → /document-release → Demo Director: pitch + Slack post
```

---

## ETHOS (4 принципи — кожен агент знає)

- **Boil the Lake** — повна фіча, не 80%. З AI 20% коштує секунди.
- **Search Before Building** — перевір що є (research/INDEX.md), перш ніж писати.
- **Demo > Perfect** — робочий UI б'є чисту архітектуру.
- **Cut > Debate** — сумніваєшся → ріж фічу.

---

## Як систематизувати роботу (для кожного агента)

1. **Початок задачі:** прочитай свій experience log (`.claude/agents/memory/<твій>-experience.md`)
   + relevant файл з `research/INDEX.md`
2. **Під час:** якщо фаза = gstack-скіл → виклич скіл, не імпровізуй
3. **Блок:** `/investigate` ПЕРШ ніж гадати причину
4. **Кінець задачі:** допиши урок у свій experience log
5. **Кінець сесії:** `/context-save` + онови `LOG.md` секцію "ЗАРАЗ"

Це не бюрократія. Це те, що дозволяє наступній сесії (або іншому агенту) підхопити з нуля.
