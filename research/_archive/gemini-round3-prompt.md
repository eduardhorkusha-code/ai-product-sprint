# Deep Research Round 3 — Gemini
**Дата:** 2026-06-04  
**Фокус:** те що виникло після другого PDF — заповнюємо конкретні прогалини

---

## Промпт для Gemini Deep Research

```
I'm preparing for a 1-day corporate mobile hackathon on June 7, 2026.
Stack: Expo SDK 53 + React Native + NativeWind v4 + Supabase + Claude Code.

Your previous research gave me 5 micro-SaaS product ideas (Specsheet Mobile, 
PermitSync, Podscriptor, Offline Field Inspector, Card Saver). 
But ALL of them take 36-60 hours to build — too long for a 1-day demo.

I also found the Callstack agent-skills repo and 'use dom' directive.

I need research on 4 NEW specific gaps:

---

QUESTION 1: Callstack agent-skills — what's actually inside?

Research the GitHub repo: https://github.com/callstackincubator/agent-skills

Specifically:
- What React Native patterns does it encode (list all skills/files)?
- What common hallucinations does it prevent Claude Code from making?
- How exactly do you install it in a project and wire it to CLAUDE.md?
- Which skills are most valuable for a 1-day hackathon vs long-term projects?
- Are there similar skill packs for Expo specifically (not just React Native)?

---

QUESTION 2: Real hackathon-winning apps that use 'use dom' + Reanimated

Find specific examples of mobile apps that:
- Use Expo's 'use dom' directive for charts/visualizations
- Were built in < 24 hours OR presented as demos
- Combined native haptics + web visualization for a wow effect

For each: what the visualization showed, how the native↔web bridge was used,
and the approximate build time.

Also: what are the most impressive data visualizations achievable with 
'use dom' + Victory Native + D3.js in under 4 hours?

---

QUESTION 3: Corporate productivity apps that WIN internal company votes

The format: 30 employees vote in Slack for the most useful tool.
These are Ukrainian tech company employees: HR, PM, Finance, Legal, Marketing.

Research:
- What type of apps consistently win internal productivity tool votes in companies?
- What features make employees actually VOTE (not just say "nice") in Slack polls?
- Are there documented cases of internal hackathon winners at tech companies 
  (any company) and what made their demo compelling?
- What is the "social proof moment" in a mobile app demo that converts skeptics 
  into voters? (Examples: seeing real data, collaborative features, instant value)

---

QUESTION 4: NativeLaunch.dev templates — what do they include?

Research: https://nativelaunch.dev and https://nativelaunch.dev/expo-supabase-template

Specifically:
- What's included in the free vs paid tier?
- Does the Expo + Supabase template include SecureStore auth (not AsyncStorage)?
- Does it include Expo Router v3 with typed routes?
- What's the actual setup time from clone to first screen?
- Compare with toyamarodrigo/expo-router-template (GitHub free) — which is better 
  for a 1-day hackathon?

Also find 2-3 other production-quality Expo starter templates released in 2025-2026
that include: auth + Supabase + NativeWind + typed routes + animations.

---

Output format:
- Question 1: list every skill in callstackincubator/agent-skills with 1-line description
- Question 2: minimum 3 specific app examples with code pattern summary
- Question 3: ranked list of app features by "vote conversion power" with evidence
- Question 4: comparison table of top 4 templates with columns:
  [name, free/paid, SecureStore, Expo Router v3, NativeWind v4, Supabase, 
   animations, setup time, hackathon suitability /10]
```
