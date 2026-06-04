# Промпти для ChatGPT та Grok
**Оновлено:** 2026-06-04  
**Логіка:** кожна модель сильна в різному — даємо кожній те, де вона виграє

---

## Навіщо питати різні AI

| AI | Сильна сторона | Що запитуємо |
|----|---------------|-------------|
| **Gemini Deep Research** | Академічний огляд, структура, джерела | Інфраструктура, трек-дорожки (вже зроблено) |
| **ChatGPT o3 / o4** | Глибокий технічний аналіз, архітектурні рішення, детальний код | Supabase схеми, edge cases, тестування |
| **Grok** | Real-time X/Twitter, що будують зараз, community insights | Тренди 2026, що виграє хакатони, viral demo patterns |
| **Claude** | Довгий контекст, нюанси, якість коду | Реалізація, рев'ю, рефакторинг |
| **Perplexity** | Швидкий пошук + джерела | Конкретні бібліотеки, актуальність пакетів |

---

## ПРОМПТ #1 — ChatGPT (технічна глибина)

```
I'm preparing for a 1-day mobile app hackathon on June 7, 2026. Stack: React Native + Expo SDK 53, 
Expo Router v3, NativeWind v4, Zustand, AsyncStorage or Supabase. 
Solo developer + 3 AI coding agents. Target audience: non-technical corporate employees.

I need deep technical answers to 5 specific questions:

---

QUESTION 1: Supabase schema design for the 3 most common hackathon apps

Design optimal PostgreSQL schemas for:
a) Team mood/pulse checker (anonymous votes, realtime results)
b) Habit tracker with streak tracking (multi-user, shared visibility)
c) Personal win journal (private entries, shareable cards)

For each schema provide:
- Full SQL CREATE TABLE statements
- Row Level Security policies (for anon/authenticated users)
- The 2-3 supabase-js queries needed for the core feature
- Enable realtime command: ALTER PUBLICATION supabase_realtime ADD TABLE...

---

QUESTION 2: The exact error patterns that kill Expo projects during demos

List the top 10 most common runtime errors in React Native + Expo that appear:
- During live demos (not in development)
- Specific to Expo Go (not dev builds)
- Specific to NativeWind v4

For each error: exact error message, root cause, fix in < 5 lines.

---

QUESTION 3: FlashList vs FlatList vs SectionList — decision matrix

For a hackathon app with:
- Lists of 5-50 items
- Animated items (Reanimated entering/exiting)
- Swipe-to-delete gesture
- Real-time additions at the top

Which list component to use and why? Give me the exact performance-safe 
implementation pattern combining FlashList + Reanimated + Gesture Handler.

---

QUESTION 4: Zustand store architecture for a habit tracker

Design the complete Zustand store for a habit tracker app with:
- Habits with completedDates array
- Tasks linked to habits (optional)
- Statistics (streak, completion rate, last 30 days)
- Persistence via AsyncStorage (zustand/middleware persist)

Include: full TypeScript interfaces, store definition, computed selectors 
(streak calculation, today's completion %, last 7 days data for chart).
Make it production-quality, not a tutorial skeleton.

---

QUESTION 5: React Native performance — what actually matters in 8 hours

In a hackathon context, which performance optimizations are:
a) CRITICAL (skip these = demo crashes/lags)
b) OPTIONAL (nice to have but skip if time-constrained)
c) TRAP (developers think they need it but don't)

Be specific to React Native + Expo Go + iPhone demo scenario. 
Include: useMemo/useCallback when actually needed, image optimization 
that matters for demo, list rendering optimizations that are worth doing.

---

Output format: for each question, give me production-ready code 
(no placeholders, no "..."), TypeScript strict, Expo SDK 53 compatible.
Label each code block with filename where it should live.
```

---

## ПРОМПТ #2 — ChatGPT (UX + Demo Psychology)

```
I'm building a mobile productivity app for a 1-day corporate hackathon. 
The judges are non-technical employees (HR, Finance, Legal, Marketing, PM) 
at a Ukrainian tech company. Voting happens on Slack — team picks the most 
useful tool they'd actually use at work.

Give me research-backed answers on:

QUESTION 1: What UX patterns make corporate employees say "I need this" 
in the first 30 seconds of seeing a mobile app demo?

Based on mobile UX research (Nielsen Norman Group, Baymard, etc.):
- Top 5 cognitive triggers that make non-technical users perceive an app as "professional"
- Which interaction patterns (gestures, animations, transitions) feel "native" vs "web-y" to iPhone users
- What makes a productivity app feel "safe" to recommend to your boss

QUESTION 2: Demo video psychology — what makes corporate Slack voters click "vote"?

Based on product demo research and viral product launches:
- Optimal demo video length for a Slack channel vote (attention span data)
- The exact emotional arc that converts viewers to voters
- Screen recording vs live demo: which wins in a synchronous vote
- 3 specific app demos from 2023-2025 that went viral inside companies — what made them work

QUESTION 3: The "10x better" principle for corporate productivity apps

For each of these pain points, what would a 10x better solution look like 
(not incremental improvement):
- Weekly status reports (currently: writing in Notion/Google Docs)
- Mood/energy check-ins for teams (currently: Slack poll or nothing)
- Personal daily wins tracking (currently: nothing, people just forget)
- Expense tracking on mobile (currently: screenshot + Telegram + accountant)

For each: what's the current friction, what's the minimum viable "10x moment",
and what's the fastest demo-able version.
```

---

## ПРОМПТ #3 — Grok (тренди і community insights)

```
Search X (Twitter) and current web for real-time insights. Today is June 2026.

QUESTION 1: What are developers actually building at mobile hackathons in 2026?

Search X for recent posts about:
- #mobilehackathon #expohackathon #reactnativehackathon (last 3 months)
- What tech stacks are winning
- What ideas are getting the most audience reaction
- Any "I won a hackathon with this" posts from 2025-2026

Find 5-10 specific examples with links if possible.

---

QUESTION 2: What's trending in Expo/React Native community RIGHT NOW (June 2026)?

Search for:
- New Expo SDK 53 features developers are excited about
- NativeWind v4 pain points developers complain about
- New React Native libraries getting attention in last 60 days
- Any breaking changes in Expo Router v3/v4 people are hitting

---

QUESTION 3: What productivity apps went viral on X in 2025-2026?

Find apps that:
- Launched as demos/MVPs and got traction
- Were built in < 1 week by solo developers
- Got significant engagement in product communities (ProductHunt, Indie Hackers, X)

For each: what made it resonate, how it was demoed, what was the core "wow moment".

---

QUESTION 4: Corporate AI tools adoption trends in 2025-2026

Search for:
- What internal tools are companies building with AI for their employees
- Which departments (HR, Finance, Legal) are seeing most AI adoption in mobile tools
- What do employees actually use vs what gets built and ignored
- Any data on Slack-based tool voting or adoption in corporate settings

Give me specific examples, not general trends.
```

---

## ПРОМПТ #4 — Grok (конкуренти буткемпу)

```
Search for insights about:

1. SKELAR company bootcamps or internal hackathons — any public posts, 
   LinkedIn mentions, or news about their "Claude Bootcamp" or 
   "Creating Digital Product" events

2. What apps Ukrainian tech company employees tend to vote for in internal 
   Slack channels — any patterns from similar corporate hackathons in 2024-2025

3. "Atomic Habits" app implementations — what's the most-cloned/referenced 
   habit tracker implementation in React Native on GitHub (by stars, forks, issues)
   
4. What makes a hackathon demo video get reshared on LinkedIn — 
   specific examples from Eastern European tech community 2024-2025

5. Expo + Claude Code combinations — any developers sharing their experience 
   using AI agents (Claude Code, Cursor, etc.) in hackathons. 
   What worked, what didn't?
```

---

## ПРОМПТ #5 — Perplexity (технічна актуальність)

```
Quick research tasks — need specific, factual, up-to-date answers:

1. Is @gorhom/bottom-sheet compatible with Expo SDK 53 and React Native New Architecture? 
   What's the latest version and any known issues?

2. What's the current status of react-native-view-shot with Expo SDK 53? 
   Any alternative if it doesn't work?

3. Is victory-native or react-native-chart-kit more actively maintained in 2026? 
   Which works better with Expo Go (no dev build)?

4. What's the fastest way to implement a GitHub-style contribution calendar 
   in React Native in 2026 — any ready library or component?

5. NativeWind v4 + Expo SDK 53: what are the current known bugs and workarounds?
   Check GitHub issues opened in last 60 days.

6. nanoid/non-secure in Expo — any issues with the crypto module in 2025-2026?
   Best alternative for generating IDs in React Native?
```
