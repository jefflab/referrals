# Hermes LLM Wiki Plan

## Summary

Build the real estate referrals knowledge base around Hermes' official support for Karpathy's LLM Wiki pattern.

The core idea is that the wiki is not just a note store or retrieval index. It is a persistent, compounding Markdown knowledge base that Hermes actively maintains. Raw sources remain immutable. The wiki becomes the synthesized layer. A schema file teaches Hermes how to keep the knowledge base coherent over time.

In this model:

- Telegram is the capture and conversation layer.
- `referrals-kb` is the durable memory layer.
- Quartz is the private read interface for iPhone and MacBook Pro.
- Tailscale is the private network path to the Quartz site.
- Obsidian is the local Markdown editor on the MacBook Air, and optional on MacBook Pro for power editing.
- GitHub is the history, review, and collaboration layer.
- `referrals-code` is the separate implementation repo for pages, scripts, and code changes.

## Why This Approach

Karpathy's LLM Wiki pattern fits this project well because the hard part is not simply storing ideas. The hard part is maintaining the relationships among campaigns, audiences, geographies, referral partners, assumptions, decisions, and open questions as the business evolves.

Hermes already has a bundled `research/llm-wiki` skill that supports this style of wiki directly. That means the system should use Hermes' existing wiki conventions instead of inventing a parallel structure.

The main task is to create a referrals-specific `SCHEMA.md` so Hermes knows how to maintain the wiki for this business.

Start with the smallest taxonomy that makes filing obvious. Add more top-level folders only after the wiki shows repeated pressure for them.

## Initial Use Cases

The starting taxonomy should support these use cases without making Jeff, Stacy, or Hermes think too hard about where a page belongs:

1. Capture Telegram discussions between Jeff and Stacy into durable knowledge.
2. Turn a raw idea into a campaign note, such as Cal Poly alumni moving back to San Luis Obispo.
3. Track audience segments inside campaign or note pages, such as Cal Poly parents, alumni, retirees, investors, or outbound California movers.
4. Track markets and origin-destination pairs, such as Bay Area to San Luis Obispo or California to Tennessee.
5. Record referral partner needs and candidate agents by market.
6. Preserve source material, articles, transcripts, screenshots, and web findings.
7. Compare campaign opportunities side by side.
8. Convert approved campaign ideas into GitHub Issues or PRs in `referrals-code`.
9. Keep a running log of decisions, ingests, and wiki changes.
10. Browse the evolving knowledge base from iPhone and MacBook Pro through Quartz over Tailscale.

## Recommended Repos

### `referrals-kb`

Private GitHub repo and Obsidian vault.

Purpose:

- Campaign ideas
- Audience notes
- Geography notes
- Referral partner research
- Telegram summaries
- Strategic decisions
- Open questions
- Source material
- Synthesized wiki pages

### `referrals-code`

Private GitHub repo for implementation work.

Purpose:

- SEO pages
- Landing pages
- Scripts
- Automation
- Data pipelines
- Site code
- Pull requests from Hermes

Hermes should contribute to this repo like another developer: create a branch, commit changes, open a PR, and wait for Jeff or Stacy to approve.

## Recommended Directory Structure

```text
referrals-kb/
  SCHEMA.md
  index.md
  log.md

  raw/
    telegram/
    sources/

  campaigns/
  markets/
  partners/
  notes/
```

## Folder Meanings

### `raw/`

Immutable source material. Hermes can add sources here, but should not rewrite the original source files after capture.

Examples:

- Telegram conversations
- Pasted notes
- Web articles
- Research transcripts
- Screenshots or downloaded assets

`raw/telegram/` is for Telegram-derived source records.

`raw/sources/` is for everything else that should be preserved as source material.

### `campaigns/`

Specific referral campaigns that might be tested.

Examples:

- Cal Poly alumni who may want to move back to San Luis Obispo
- Cal Poly student parents who may want to buy investment homes
- People leaving California for Tennessee
- People leaving Tennessee for California
- Bay Area residents considering San Luis Obispo

Campaign pages can include audience, offer, channel, and next-action details directly. Do not create separate audience, offer, or channel pages unless the same idea keeps recurring across multiple campaigns.

### `markets/`

Geographies, destination markets, and origin-destination pairs.

Examples:

- San Luis Obispo
- San Francisco Bay Area to San Luis Obispo
- California to Tennessee
- Tennessee to California

Use `markets/` instead of separate `geographies/` and `audiences/` folders at first because the business is really about referral market opportunities.

### `partners/`

Candidate or approved referral agents and partner relationships.

Examples:

- San Luis Obispo buyer agents
- Tennessee relocation specialists
- Bay Area listing agents

### `notes/`

Everything that does not yet deserve its own top-level folder.

Examples:

- Concepts
- Comparisons
- Decisions
- Open questions
- Query answers
- Offers
- Channels
- Audience sketches
- Entity notes

Promote a note into a more specific folder only when Jeff or Stacy can predict where that kind of page belongs without thinking.

## Wiki Primitives

Folders are only storage locations. The more important design is the small set of page types Hermes should understand.

Use these primitives:

| Primitive | Folder | Intended to track |
| --- | --- | --- |
| `source` | `raw/` | Raw evidence: Telegram threads, URLs, social posts, screenshots, search results, transcripts. |
| `campaign` | `campaigns/` | A repeatable referral play: audience, pitch, channel, market, partner, next action. |
| `market` | `markets/` | A geography or origin-destination pair: San Luis Obispo, San Diego to San Francisco, California to Tennessee. |
| `partner` | `partners/` | The receiving agent or referral partner in a market. |
| `signal` | `notes/` | A clue that someone may have referral intent, such as "thinking of moving," "kid going to Cal Poly," or "new job in SF." |
| `opportunity` | `notes/` | A human-review candidate: person, group, thread, post, or cluster worth considering. |
| `relationship` | `notes/` | A trust path, such as Stacey knows Alice, Alice knows Bob, and Bob may be moving. |

The durable model is:

```text
Sources create signals.
Signals create opportunities.
Opportunities roll up into campaigns.
Campaigns target markets.
Markets need partners.
Relationships create trust paths.
```

Keep audience, offer, channel, message, and experiment details inside these primitive pages until they clearly need their own top-level structure.

## Hermes Configuration

Set Hermes' wiki path to the knowledge repo:

```text
WIKI_PATH=/path/to/referrals-kb
OBSIDIAN_VAULT_PATH=/path/to/referrals-kb
```

`WIKI_PATH` tells Hermes where to maintain the wiki.

`OBSIDIAN_VAULT_PATH` points Obsidian at the same directory for local review and power editing on the MacBook Air.

Most edits should happen by talking to Hermes in Telegram. Hermes then updates the Markdown files, keeps links/index/log current, and commits or prepares reviewable changes as needed.

## Access Model

The MacBook Air is the canonical host for the Hermes-maintained wiki.

Jeff and Stacy should not need iCloud on the MacBook Air, and the wiki should not be exposed to the public internet.

Use this access model:

```text
MacBook Air
  Hermes edits referrals-kb Markdown
  Obsidian can open the local vault
  Quartz renders the vault as a static website
  Tailscale Serve exposes Quartz privately to the tailnet

iPhone
  Tailscale app
  Safari opens the private Quartz URL
  Telegram is the primary editing/input path

MacBook Pro
  Tailscale app
  Browser opens the private Quartz URL
  Optional Git clone + Obsidian for power editing
```

The first version should optimize for mobile viewing, not mobile file editing. If Jeff or Stacy wants to change the wiki from iPhone, they should message Hermes in Telegram and let Hermes make the Markdown edit.

### Quartz Viewing

Quartz is the read interface for Jeff and Stacy.

Use Quartz because it can render Obsidian-style Markdown, links, backlinks, and graph-like navigation into a browser-friendly site. This makes the wiki easy to browse from iPhone and MacBook Pro without needing Obsidian Sync, iCloud Drive, or direct filesystem access.

The expected private URL should be a Tailscale hostname such as:

```text
https://hermes-macbook-air.<tailnet>.ts.net
```

Use Tailscale Serve, not Tailscale Funnel. Serve keeps access inside the private tailnet. Funnel is for public internet exposure and should not be used for this wiki.

### Editing Paths

Primary editing path:

```text
Jeff/Stacy -> Telegram -> Hermes -> Markdown files -> Quartz refresh
```

Secondary editing path:

```text
MacBook Air -> Obsidian -> Markdown files
```

Optional power-user editing path:

```text
MacBook Pro -> Git clone referrals-kb -> Obsidian -> commit/push
```

Avoid iCloud as the first sync mechanism. A dummy iCloud account could technically share a folder with a real Apple account, but it adds Apple identity, sync conflicts, and agent-permission ambiguity. Git plus Quartz plus Tailscale is easier to reason about.

## Telegram Workflow

Telegram is the conversational intake layer for Jeff and Stacy.

When Jeff or Stacy discusses a useful idea in Telegram, Hermes should:

1. Save the relevant exchange into `raw/telegram/`.
2. Search `index.md` and the wiki for related existing pages.
3. Update existing pages where possible.
4. Create new pages only when the idea is important enough to preserve.
5. Add `[[wikilinks]]` between related pages.
6. Update `index.md`.
7. Append to `log.md`.
8. If the idea becomes executable, create a GitHub Issue or PR in `referrals-code`.

Example Telegram idea:

> What about Cal Poly parents buying investment houses in SLO?

Hermes may update or create:

```text
raw/telegram/2026-05-25-cal-poly-parent-investment-homes.md
campaigns/cal-poly-parents-investment-homes.md
markets/san-luis-obispo.md
notes/student-housing-investment.md
```

## Outbound Research Path

The referral business should not depend only on traditional SEO where pages are published and the team waits for inbound traffic. Hermes should also run outbound research loops that look for public intent signals, community discussions, and partner opportunities.

The goal is research and prioritization first, not automated outreach. Hermes should find candidate opportunities, preserve sources, summarize why they matter, and queue human review before anyone contacts a person or group.

### Supported Hermes Tools

Use Hermes' official capabilities in this order:

1. `x_search` for X / Twitter posts, profiles, and threads.
2. `web_search` for broad discovery across public web results.
3. `web_extract` for specific URLs found through search.
4. Browser automation for JS-heavy, authenticated, or group-based sites.
5. MCP or custom Hermes plugins only after a source becomes important enough to justify more setup.

Hermes has first-class `x_search` support. For Facebook, Facebook Groups, LinkedIn, Instagram, and Reddit, assume the first version uses web search, URL extraction, and browser automation rather than native platform APIs.

### Research Sources

Start with these source types:

- X / Twitter discussions about moving, relocation, schools, jobs, retirement, or housing.
- Reddit posts in local, university, relocation, investing, and real estate communities.
- Facebook Groups, especially local groups, alumni groups, parent groups, relocation groups, and housing groups.
- LinkedIn posts about relocation, layoffs, new jobs, remote work, alumni networks, and local professional communities.
- Instagram posts and comments around local lifestyle, student housing, alumni events, and relocation.
- Alumni association pages and event listings.
- Cal Poly parent and alumni community pages where accessible.
- BiggerPockets and real estate investor forums.
- Meetup and Eventbrite pages for local relocation, alumni, investor, and community events.
- Local news, school, city, county, and chamber-of-commerce pages.
- YouTube videos and comments about moving to, leaving, or investing in a market.

### Initial Research Loops

Hermes should begin with a few recurring research loops:

1. Cal Poly alumni returning to San Luis Obispo.
2. Cal Poly student parents interested in buying student housing or investment homes.
3. People considering a move from California to Tennessee.
4. People considering a move from Tennessee to California.
5. Bay Area residents considering San Luis Obispo, Central Coast, or smaller California markets.
6. Candidate referral partner agents in San Luis Obispo and Tennessee.

Each loop should produce a short research note, not a large scrape.

### Outbound Research Workflow

For each research loop, Hermes should:

1. Define the hypothesis.
2. Search X / Twitter with `x_search` when relevant.
3. Search the public web with `web_search`.
4. Extract only useful URLs with `web_extract`.
5. Use browser automation only when a page requires rendering or login.
6. Save source records into `raw/sources/`.
7. Summarize durable findings into `campaigns/`, `markets/`, `partners/`, or `notes/`.
8. Add links between the campaign, market, partner, and source pages.
9. Flag candidate opportunities for Jeff/Stacy review.
10. Append the work to `log.md`.

### Source Capture Template

Use this shape for outbound source records in `raw/sources/`:

```md
---
type: source
source_kind: social-post | thread | group | profile | article | forum | video | event | search-result
platform:
url:
captured_at:
research_loop:
status: raw
tags:
  - outbound-research
---

# Source Title

## Why Captured

## Relevant Excerpt Or Summary

## Possible Referral Signal

## Related Pages
```

### Candidate Opportunity Template

Use `notes/` for early candidate opportunities that are not yet campaign pages:

```md
---
type: opportunity
status: review
source_platform:
market:
campaign:
confidence: low
tags:
  - outbound-research
  - human-review
---

# Opportunity Name

## Signal

## Why It May Matter

## Source Links

## Suggested Human Review

## Possible Next Action

## Related
```

### Research Safety Rules

Hermes should follow these rules for outbound research:

1. Do not automate outreach in the first version.
2. Do not send DMs, comments, friend requests, connection requests, or group posts without explicit approval.
3. Do not bypass logins, paywalls, CAPTCHAs, rate limits, or platform restrictions.
4. Do not store private personal data unless Jeff or Stacy explicitly approves the source and purpose.
5. Prefer public, citeable URLs.
6. Treat social posts as signals, not facts.
7. Keep the wiki focused on patterns, opportunities, and review queues rather than accumulating personal dossiers.
8. Mark low-confidence findings as low confidence.
9. Preserve source links so Jeff and Stacy can inspect context before acting.
10. Escalate sensitive or ambiguous outreach decisions to human review.

### Hermes Setup For Outbound Research

On the MacBook Air, enable:

```text
Telegram Gateway
Hermes LLM Wiki
Web Search / Web Extract
X Search
Browser Automation
GitHub MCP
```

For X / Twitter search, configure either xAI OAuth or an xAI API key:

```bash
hermes auth add xai-oauth
```

or:

```text
XAI_API_KEY=...
```

For general web research, configure a web backend through `hermes tools`. Prefer Firecrawl or Tavily first because they support search and extraction. Exa is also useful for semantic search.

For Facebook, LinkedIn, Instagram, Reddit, and other JS-heavy sources, configure browser automation and use manual login where required. Hermes should report login, 2FA, CAPTCHA, or access blockers to Jeff/Stacy instead of guessing.

## Validation Examples

These examples should be used to validate whether the wiki structure is simple enough and expressive enough.

### Cal Poly Alumni Returning To SLO

Scenario:

Stacey wants to reach Cal Poly alumni who may want to move back to San Luis Obispo. If someone is interested, Stacey would refer them to her business partner in SLO.

Relevant pages:

```text
campaigns/cal-poly-alumni-return-to-slo.md
markets/san-luis-obispo.md
partners/staceys-slo-business-partner.md
notes/cal-poly-alumni.md
notes/move-back-to-college-town.md
raw/sources/cal-poly-alumni-thread-2026-05-26.md
```

The campaign page should track:

```md
# Cal Poly Alumni Returning To SLO

## Audience
Cal Poly alumni who may want to move back to San Luis Obispo.

## Market
[[markets/san-luis-obispo]]

## Partner
[[partners/staceys-slo-business-partner]]

## Signals
- Mentions missing SLO
- Kids leaving home
- Remote work flexibility
- Retirement or semi-retirement
- Posts about visiting Cal Poly or the Central Coast

## Channels
- Facebook alumni groups
- LinkedIn alumni search
- Cal Poly event pages
- X / Twitter search
- Reddit local and Cal Poly discussions

## Offer
"Have you ever thought about moving back to SLO? Stacey has a trusted local partner there."

## Next Actions
- Find public alumni discussions.
- Identify warm contacts first.
- Draft message variants for human approval.
```

This validates the basic pattern: a `campaign` targets a `market`, uses outbound `sources`, looks for `signals`, and routes interested people to a `partner`.

### San Diego To San Francisco

Scenario:

Stacey wants to identify people who currently live in San Diego but want to move to San Francisco. Stacey would refer them to her former mentor in SF.

Relevant pages:

```text
campaigns/san-diego-to-san-francisco.md
markets/san-diego-to-san-francisco.md
partners/staceys-former-sf-mentor.md
notes/san-diego-residents-moving-to-sf.md
raw/sources/reddit-san-diego-moving-to-sf-thread.md
```

The market page should track:

```md
# San Diego To San Francisco

## Why People Move
- New jobs
- Return to the Bay Area
- Tech career moves
- Family proximity
- School or professional opportunity

## Why It Matters
Stacey can refer buyers or sellers to her former mentor in San Francisco.

## Referral Partner
[[partners/staceys-former-sf-mentor]]

## Signals
- "Moving to SF"
- "Relocating to Bay Area"
- "New job in San Francisco"
- "Should I rent or buy in SF?"
- "Leaving San Diego for Bay Area"

## Related Campaigns
[[campaigns/san-diego-to-san-francisco]]
```

The partner page should track:

```md
# Stacey's Former SF Mentor

## Market
San Francisco / Bay Area

## Relationship
Former mentor of Stacey.

## Best Fit
San Diego residents moving to San Francisco or the Bay Area.

## Trust Notes
High-trust existing relationship.

## Related Campaigns
[[campaigns/san-diego-to-san-francisco]]
```

This validates that origin-destination opportunities belong in `markets/`, while the referral relationship belongs in `partners/`.

### Friends Of Friends Outreach

Scenario:

Stacey first identifies who she already knows from Facebook and LinkedIn. Hermes segments that sphere of influence by geography, life event, or other signal. Stacey can then reach out to first-degree connections with custom messaging. For second-degree connections, Stacey can potentially name-drop the mutual connection for trust building, subject to human review.

Relevant pages:

```text
campaigns/friends-of-friends-relocation-outreach.md
notes/stacey-sphere-of-influence.md
notes/relationship-stacey-alice-bob.md
notes/network-segment-bay-area-cal-poly-parents.md
raw/sources/facebook-network-notes.md
raw/sources/linkedin-network-search-notes.md
```

The campaign page should track:

```md
# Friends Of Friends Relocation Outreach

## Thesis
People are more likely to respond when Stacey can reach them through a mutual connection or shared context.

## Audience
People connected to Stacey's sphere of influence who may have a real estate transition.

## Trust Path
- First degree: Stacey knows them directly.
- Second degree: Stacey knows someone who knows them.
- Shared context: Cal Poly, neighborhood, family stage, career move, local market.

## Segmentation
- Geography: SLO, SF, San Diego, Tennessee
- Life event: new job, retirement, kid in college, empty nest, divorce, downsizing
- Affinity: Cal Poly, former coworkers, parents, alumni, local groups

## Signals
- "Moving"
- "New job"
- "Kid going to Cal Poly"
- "Thinking about buying"
- "Leaving California"
- "Need a realtor"
- "Looking for neighborhood advice"

## Outreach Rule
No automated outreach. Hermes queues candidates and drafts custom messages for human approval.
```

A relationship page should track:

```md
# Relationship: Stacey -> Alice -> Bob

## Known Person
Alice

## Candidate Person
Bob

## Relationship Path
Stacey knows Alice. Alice appears connected to Bob.

## Possible Trust Hook
"Stacey and Alice know each other through..."

## Signal
Bob posted about relocating to San Francisco.

## Suggested Action
Ask Stacey whether Alice is an appropriate person to mention or ask for an introduction.
```

This validates the `relationship` primitive. The wiki should track trust paths and review queues, not become a private-data-heavy CRM.

## Page Template: Campaign

```md
---
type: campaign
status: seed
audience:
geography:
channel:
confidence: medium
tags:
  - campaign
---

# Campaign Name

## Audience

## Geography

## Trigger / Motivation

## Offer

## Referral Partner Fit

## Channels

## Evidence

## Risks

## Open Questions

## Next Actions

## Related
```

## Page Template: Market

```md
---
type: market
status: seed
confidence: medium
tags:
  - market
---

# Market Name

## Market Summary

## Why People Move Here

## Why People Leave

## Relevant Audiences

## Referral Partner Needs

## Campaign Ideas

## Related
```

## Page Template: Partner

```md
---
type: partner
status: candidate
geography:
confidence: low
tags:
  - partner
---

# Partner Name or Market

## Geography

## Specialty

## Referral Terms

## Trust Notes

## Contact Notes

## Related Campaigns
```

## Suggested Tag Taxonomy

```text
campaign
market
partner
signal
opportunity
relationship
offer
channel
seo
telegram-thread
decision
open-question
alumni
school-affinity
relocation
investment-property
referral-fee
compliance
outbound-research
human-review
source
```

New tags should be added to `SCHEMA.md` before use.

## Update Rules

Hermes should follow these rules:

1. Always orient first by reading `SCHEMA.md`, `index.md`, and recent `log.md` entries.
2. Keep raw sources immutable.
3. Prefer updating existing pages over creating duplicates.
4. Create new pages only when the topic is recurring, strategically important, or central to a source.
5. Every new or updated page should link to at least two related pages when possible.
6. Always update `index.md` after adding important pages.
7. Always append meaningful changes to `log.md`.
8. Record contradictions explicitly instead of silently overwriting old claims.
9. Use frontmatter consistently.
10. Keep pages scannable. Split long pages into focused subpages.

## Index Template

```md
# Wiki Index

> Content catalog for the real estate referrals knowledge base.
> Read this first to find relevant pages.
> Last updated: YYYY-MM-DD | Total pages: N

## Campaigns

## Markets

## Partners

## Notes
```

## Log Template

```md
# Wiki Log

> Append-only record of meaningful wiki activity.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete

## [YYYY-MM-DD] create | Wiki initialized
- Domain: real estate referrals
- Structure created with SCHEMA.md, index.md, and log.md
```

## Relationship To Obsidian

Use Obsidian locally because it aligns naturally with the Hermes LLM Wiki pattern:

- Plain Markdown files
- `[[wikilinks]]`
- Graph view
- YAML frontmatter
- Local browsing
- Git-friendly vault

Obsidian should not be the required mobile access path. iPhone access should use Quartz over Tailscale, and iPhone edits should usually happen through Telegram to Hermes.

## Relationship To Tailscale

Tailscale is part of the first pass because the wiki should be visible from iPhone and MacBook Pro away from the house without being public.

Use Tailscale Serve to expose the local Quartz site from the MacBook Air to Jeff and Stacy's tailnet devices. Do not use Tailscale Funnel for this wiki.

## Relationship To Code

The knowledge base should not become the code repo.

When a wiki idea becomes implementation work, Hermes should create one of:

- GitHub Issue in `referrals-code`
- branch and PR in `referrals-code`
- checklist item in a campaign page if the idea is not ready for code yet

Examples:

- Create an SEO landing page for Cal Poly alumni returning to SLO.
- Add an `llms.txt` file for referral discovery.
- Build a simple landing page generator for origin-destination campaigns.
- Add a script that turns approved campaign pages into draft SEO pages.

## First Implementation Pass

1. Create a private `referrals-kb` GitHub repo.
2. Clone it on the MacBook Air.
3. Point Hermes `WIKI_PATH` at the repo.
4. Open the same folder as an Obsidian vault on the MacBook Air.
5. Install and configure Quartz to render the vault as a local static site.
6. Install Tailscale on the MacBook Air, Jeff's iPhone, Stacey's iPhone, and Jeff/Stacey laptops that need access.
7. Use Tailscale Serve to expose the local Quartz site privately to the tailnet.
8. Confirm the Quartz site opens from iPhone Safari and MacBook Pro while away from the home network.
9. Add `SCHEMA.md`, `index.md`, and `log.md`.
10. Seed the first pages:
   - `campaigns/cal-poly-alumni-return-to-slo.md`
   - `campaigns/cal-poly-parents-investment-homes.md`
   - `campaigns/san-diego-to-san-francisco.md`
   - `campaigns/friends-of-friends-relocation-outreach.md`
   - `campaigns/california-to-tennessee-relocation.md`
   - `markets/san-luis-obispo.md`
   - `markets/san-diego-to-san-francisco.md`
   - `markets/california-to-tennessee.md`
   - `partners/staceys-slo-business-partner.md`
   - `partners/staceys-former-sf-mentor.md`
   - `notes/cal-poly-alumni.md`
   - `notes/cal-poly-student-parents.md`
   - `notes/stacey-sphere-of-influence.md`
   - `notes/relationship-stacey-alice-bob.md`
   - `notes/real-estate-referral-fee.md`
11. Configure Telegram ingestion so useful discussions are captured into `raw/telegram/`.
12. Treat Telegram as the primary editing path for iPhone and casual edits.
13. Enable Hermes outbound research tools: web search, web extract, X search, browser automation, and GitHub MCP.
14. Start the first outbound research loops:
   - Cal Poly alumni returning to San Luis Obispo
   - Cal Poly student parents buying investment homes
   - California to Tennessee relocation
   - Tennessee to California relocation
   - Bay Area to San Luis Obispo relocation
   - San Luis Obispo referral partner search
15. Let Hermes maintain the wiki through Telegram conversations and outbound research.
16. Create issues or PRs in `referrals-code` only when a campaign is ready to become implementation work.

## Guiding Question

What repeatable systems can create qualified real estate referrals at scale?
