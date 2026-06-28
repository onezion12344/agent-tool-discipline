---
name: cross-verify-search
description: >
  Cross-verification protocol for all factual queries. Use when the user asks
  about travel/tourism (opening hours, prices, locations, availability, reviews),
  historical facts, current events, product info, or any claim that could be wrong
  if sourced from a single place. Trigger: any search-dependent answer.
---

# Cross-Verification Protocol

## When this applies
Any answer that depends on searching the web for facts — especially:
- Travel/tourism: opening hours, prices, ticket policies, locations, "is it open now"
- Historical claims: dates, events, motivations
- Current information: political leadership, laws, statistics
- Product/pricing: what something costs, where to buy it

## Rule
**Never trust a single source. Never rely on internal knowledge alone.** Before presenting facts to the user:
1. **Search first, then speak.** If a claim requires factual backing, run the search before making it — not after the user questions it
2. Find the same information from at least **2 independent sources**
3. Prefer: official websites > reputable news outlets > travel blogs/social media
4. If sources disagree, present both and flag the conflict
5. If only one source found, explicitly say "⚠️ only one source confirms this"
6. **Negative claims are especially dangerous.** "Nobody knows X in China" — search before saying this. The user caught us on exactly this (2026.06.11: "中国真的没有人知道朝圣之路吗？你最好仔细搜索")

## Hard rule
**Never make unsupported sweeping claims.** If you catch yourself saying
"nobody in X knows about Y" or "X doesn't exist in Y" — STOP. Search first.
The user called this out explicitly (2026.06.11 — "胡说八道" on "中国没几个人知道朝圣之路").
When corrected by search, Forbes showed 65% growth in Chinese pilgrims. Apologize
and fix the claim immediately.

## Search-first workflow
Before answering any factual question:
1. Search (web or anysearch)
2. Read results
3. THEN speak
Never answer from memory alone. The user said "先搜再说".

## Trusted source priority
- 🥇 Official sites (`.gal`, `.es` government, venue's own `.org`/`.com`)
- 🥇 **National statistical agencies** for country-level numbers — **INE** (ine.es) for Spain, **INE** (ine.pt) for Portugal, **NBS** (stats.gov.cn) for China. These trump aggregator sites (Gitnux, Statista, Dataintelo) every time. Gitnux claimed 1,900+ Spanish albergues; INE's April 2026 official count was 1,131. (2026.06.27 lesson)
- 🥈 Major news outlets (El País, BBC, France 24, RTVE, Forbes)
- 🥉 Established travel guides (Britannica, Lonely Planet, Gronze for Camino)
- 🏅 Chinese sources (小红书, 知乎, Forbes China) alongside English/Spanish
- 🏅 Reddit/Facebook/TripAdvisor — useful for crowdsourced info but flag as "user reports"
- ❌ TikTok/Instagram alone — not sufficient for factual claims
- ❌ Agent's own memory — not sufficient without search verification

## ⚙️ Tool reuse principle (2026.06.11)
**Before building a new skill or workaround**, search for existing solutions (MCP servers,
npm packages, pre-built skills). The user explicitly prefers reusing others' work over
custom builds. Example: Google Maps access → found 3 existing MCP servers + Composio
managed OAuth, rather than building from scratch. For Maps lookups, load the
`google-maps-lookup` skill which documents these paths.

## For this user (Harry/Onezion) — MANDATORY
- **Search BEFORE answering** for any claim involving prices, hours, locations, historical dates, statistics, or "how many / how much".
- Cross-verify with **minimum 2 independent sources**. If they conflict, present both + flag it.
- **Chinese-language sources alongside English/Spanish** — always check 小红书 for travel/shopping queries. Note when Chinese sources confirm or contradict Western sources.
- He is often in Santiago de Compostela / on Camino — prioritize Galician local directories (paxinasgalegas.es) and official `.gal` sites.
- He called out unsourced claims (2026.06.11 — "胡说八道" incident). **Zero tolerance for fabricated data.**
- **Never say "probably" or "around"** without a sourced range. Give concrete numbers with attribution.
- Detailed source hierarchy for tourism queries → `references/tourism-sources.md`
- When presenting facts, note where each claim came from (source + date if relevant).
- For shopping: always include prices in €. For travel: always include opening hours + current status (open/closed now).
- He is a budget-conscious student — flag free entry options, student discounts, and cheaper alternatives.
- See `references/santiago-compostela-quickref.md` for verified Camino/Santiago practical info (museums, shops, food, stamps, politics).
- **Negative claims require search**: Never say "nobody knows X" / "X doesn't exist in Y country" without searching first. Absence of evidence is not evidence of absence. (2026.06.11: claimed China doesn't know Camino → Forbes España showed 65% growth in Chinese pilgrims)

## Reference files
- `references/santiago-souvenir-shops.md` — Verified Camino souvenir shops, prices, hours, and reviews in Santiago de Compostela (June 2026).
- `references/ine-albergues-spain-2026.md` — Official INE albergue counts by Spanish autonomous community (April 2026). Authoritative source for Spain hospitality stats; use to cross-check aggregator claims.

## Pitfalls
- **Assuming the Chinese market doesn't have something** without searching Taobao/Pinduoduo/Xiaohongshu first
- **Trusting only English/Spanish search results** when the user is Chinese — cross-check Chinese platforms
- **Presenting 8-12€ without verification** — always find at least one concrete price point from an official source before giving a range
- **Aggregator sites inflate numbers** — Gitnux, Statista, and similar aggregators sometimes report counts that contradict official government statistics. When dealing with country-level hospitality/tourism numbers, cross-check against the relevant national statistical agency (INE for Spain, NBS for China, IBGE for Brazil, etc.). Gitnux claimed 1,900+ albergues in Spain; INE's April 2026 official count was 1,131. (2026.06.27)
- **Official websites of declining orgs may be aspirational** — YHA China's website claimed "近200家" hostels while Sixth Tone reported <70 and 36氪 reported 109. When an organization is in decline and its own website contradicts independent journalism, prefer the journalism — official sites may be stale, marketing-driven, or counting defunct members. Present the conflict honestly but give more weight to independent verification. (2026.06.27)
