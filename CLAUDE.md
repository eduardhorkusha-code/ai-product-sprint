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

## Team (3 agents + Eduard as Sprint Clock)

| Agent | File | Role | Key skill |
|-------|------|------|-----------|
| **Product Brain** | [agents/product-brain.md](agents/product-brain.md) | PM + Research | `/office-hours`, `/spec` |
| **Builder** | [agents/builder.md](agents/builder.md) | Dev (mobile-first) | `/autoplan`, `/investigate` |
| **Design Eye** | [agents/design-eye.md](agents/design-eye.md) | UX/iOS design | `/ios-design-review`, `/design-review` |
| **Eduard** | Sprint Clock | Tempo + decisions | `/autoplan` to cut scope |

## Sprint Flow

### Full day (8h)
```
Hour 1: Product Brain — /office-hours → /spec (scope, user story, MVP cut)
Hour 2: Builder — skeleton + navigation (Expo + Supabase)
Hour 3-5: Builder — core features (timebox strictly; /investigate on blocks)
Hour 6: Design Eye — /design-review pass (catch AI slop)
Hour 7: Builder — polish + fixes from /design-review
Hour 8: Demo prep + /canary
```

### One-day intensive (4h, workshop format)
```
30 min: Product Brain — /office-hours → MVP = 1 core feature only
90 min: Builder — build that one feature end-to-end
30 min: Design Eye — /design-review → iOS HIG check
30 min: Polish + demo
```

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
