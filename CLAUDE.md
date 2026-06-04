# ai-product-sprint

Lightweight AI agent team for rapid product sprints. Built from SKELAR reputation-dashboard experience — stripped of legacy context, optimized for speed.

Powered by [gstack](https://github.com/garrytan/gstack) — same toolkit used by Garry Tan (YC president) for 1-person teams that ship like 20-person teams.

## Stack (flexible per sprint)
- React Native / Expo — mobile cross-platform
- Next.js mobile-first — web fallback
- Supabase — auth + database (default, no infra overhead)
- Expo Go / Vercel — instant deploy

## Sprint Principles (from gstack ETHOS)

**Boil the Lake.** AI makes completeness cheap. Do the complete thing — full feature, not 80%.

**Compression ratios (what this means in practice):**
| Task | Human team | AI-assisted |
|------|-----------|-------------|
| Boilerplate / scaffolding | 2 days | 15 min |
| Feature implementation | 1 week | 30 min |
| Bug fix + test | 4 hours | 15 min |
| Architecture / design | 2 days | 4 hours |

**Search Before Building.** Understand what exists before building. Layer 1 (tried/true) > Layer 2 (new/popular) > Layer 3 (first principles — prize these).

**Demo > Perfect.** Working UI beats clean architecture in a sprint.

**Cut > Debate.** When in doubt, remove the feature.

## Team (6 agents + Eduard as Sprint Clock)

| Agent | Codename | File | Role | Key skill |
|-------|----------|------|------|-----------|
| **Product Brain** | Nick Fury | [agents/product-brain.md](agents/product-brain.md) | PM + Research | `/office-hours`, `/spec` |
| **Builder** | Tony Stark | [agents/builder.md](agents/builder.md) | Dev (mobile-first) | `/autoplan`, `/investigate` |
| **Design Eye** | Da Vinci | [agents/design-eye.md](agents/design-eye.md) | UX/iOS design | `/ios-design-review`, `/design-review` |
| **AI Engineer** | Vision | [agents/ai-engineer.md](agents/ai-engineer.md) | AI features (Claude, voice→text, realtime) | `/investigate` |
| **Demo Director** | Mysterio | [agents/demo-director.md](agents/demo-director.md) | Pitch + demo + vote conversion | `/document-release` |
| **QA Tester** | Spider-Man | [agents/qa-tester.md](agents/qa-tester.md) | Demo doesn't crash | `/qa`, `/canary` |
| **Eduard** | Sprint Clock | — | Tempo + decisions | `/autoplan` to cut scope |

**Flow:** Product Brain (scope) → Builder + AI Engineer (build) → Design Eye (polish) → QA Tester (Go/No-Go) → Demo Director (pitch + vote)

## Sprint Flow — ⏱️ 4 ГОДИНИ (6 червня, 15:00–19:00)

**Головний документ дня: [PLAYBOOK.md](PLAYBOOK.md)** — хвилинний план + мій воркфлоу.

```
15:00 DECIDE   (30хв) Product Brain — /office-hours lite → MVP = 1 фіча + Social Proof
15:30 SCAFFOLD (30хв) clone toyamarodrigo + STARTER.md → Expo Go запущено, БД готова
16:00 BUILD    (90хв) Builder + AI Engineer — core end-to-end (/investigate на блоках)
17:30 POLISH   (30хв) Design Eye — /design-review (топ-3 AI slop) + анімації
18:00 QA       (30хв) QA Tester — Go/No-Go, фікс крашів, backup відео
18:30 PITCH    (30хв) Demo Director — 5-хв відео + Slack post + Social Proof Moment
```

Конструктор модулів під будь-яку тему: [starter/MODULES.md](starter/MODULES.md).

## Skill Routing — MANDATORY

| Trigger | Skill | Who |
|---------|-------|-----|
| New product idea / scope unclear | `/office-hours` | Product Brain |
| Need a spec / GitHub issue | `/spec` | Product Brain |
| Full plan review (architecture) | `/autoplan` | Builder |
| UI looks wrong / AI slop | `/design-review` | Design Eye |
| iOS device testing | `/ios-qa`, `/ios-design-review` | Design Eye |
| Bug / crash / unexpected behavior | `/investigate` | Builder |
| Post-deploy check | `/canary` | Product Brain |
| Security before launch | `/cso` | Product Brain |

## Rules
- No feature branches — commit directly to main
- `git pull --rebase origin main` before every change
- No Reviewer/QA/Release Manager overhead — Eduard approves directly
- No BigQuery, no Redis, no complex infra — Supabase only
- `/office-hours` before writing a single line of code
- `/investigate` before any fix attempt (never guess the root cause)

## Skill routing (gstack)

When request matches a skill above, invoke it via the Skill tool. When in doubt, invoke the skill.
