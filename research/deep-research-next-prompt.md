# Deep Research Prompt — Round 2
**Для:** Google Gemini Deep Research / Perplexity Deep Research  
**Мета:** Підготовка до перемоги на SKELAR × Claude Bootcamp "Creating Digital Product"

---

## Промпт для Deep Research

```
Research task: Winning strategy for a 1-day mobile app hackathon using Claude Code + Expo

Context:
- Competition: SKELAR internal bootcamp "Creating Digital Product", June 7 2026
- Format: 6 cross-functional teams, 1 day to build a mobile productivity app
- Stack: React Native + Expo SDK 51+, Expo Router v3, NativeWind v4, Supabase, Claude Code CLI
- Judge criteria: working demo, product quality, design polish, real-world usefulness
- Team: 1 developer (Eduard) + 3 AI agents (Product Brain PM, Builder dev, Design Eye UX)
- Voting: teammates vote in Slack channel — so demo video and presentation matter

I need deep research on 4 questions:

---

QUESTION 1: What productivity app niches are both buildable in 1 day AND impressive to a non-technical audience?

Specifically:
- Which categories (habit tracker, task manager, focus timer, journal, meeting notes, etc.) have the best ratio of: demo wow-factor vs build complexity?
- What is the single "aha moment" UI interaction in each category that makes judges say "I want this"?
- Which categories are oversaturated and will feel generic vs which feel fresh in 2026?
- What real pain points do Ukrainian office workers (HR, PM, Finance, Legal, Marketing roles) have that a 1-day app could address?

---

QUESTION 2: What are the exact Expo + React Native patterns that save the most time in a hackathon?

Specifically:
- Which Expo SDK 51 built-in components replace 2-3 hours of custom code (e.g., bottom sheets, modals, haptics)?
- What are the fastest animation patterns in React Native Reanimated 3 that look impressive but take <30 min?
- Which NativeWind v4 utility combinations produce the best-looking UI out of the box?
- What are the 3 most common Expo Go crashes/bugs during demos and how to avoid them?
- What is the fastest way to add Supabase real-time sync to a list in Expo?

---

QUESTION 3: How do top hackathon teams structure their day to maximize demo quality?

Specifically:
- What is the optimal time allocation for a 1-person team over 8 hours: skeleton → core feature → design → polish → demo prep?
- What scope decisions do winning hackathon teams consistently make (what do they cut)?
- How do the best demo videos structure their 60-90 second product walkthrough?
- What makes a Slack-based team vote go viral (what formats get most engagement — video, GIF, screenshot)?

---

QUESTION 4: What Claude Code workflows give the biggest speed advantage during the build day?

Specifically:
- What is the most effective way to use Claude Code agents in parallel during Expo development (e.g., one agent writes components while another debugs)?
- Which /skills or custom prompts are most valuable to prepare before the hackathon (not during)?
- How should CLAUDE.md be structured for an Expo project to maximize Claude Code's accuracy?
- What are the known failure modes when using Claude Code for React Native (hallucinated APIs, wrong Expo SDK versions, etc.) and how to prevent them?

---

Output format I need:
1. For each question: a ranked list of specific, actionable recommendations (not general advice)
2. For Question 1: include a comparison table of 5 app ideas with columns: [idea, wow-factor/10, build-time estimate, judge appeal, unique angle]
3. For Question 2: include ready-to-paste code snippets for the top 3 time-saving patterns
4. For Question 3: include a specific hour-by-hour schedule template for 8-hour build day
5. For Question 4: include a ready-to-use CLAUDE.md template section for Expo projects

Sources to prioritize: Expo documentation (2025-2026), React Native community repos, hackathon post-mortems, Claude Code official docs.
```

---

## Чому цей промпт (для Едуарда)

Перший ресерч від Gemini закрив **інфраструктурне питання** — як налаштувати Claude Code, MCP, скіли. Це тепер збережено в `research/gemini-bootcamp-infrastructure.md`.

Цей раунд закриває **стратегічне питання** — що будувати і як виграти. Чотири фокуси:
1. **Вибір продукту** — не починати кодити без validated idea
2. **Expo speed patterns** — конкретні компоненти і трюки що економлять години
3. **Тайминг дня** — структура 8 годин, де різати scope
4. **Claude Code під Expo** — специфічні gotchas і як підготувати агентів заздалегідь
