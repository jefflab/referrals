# LLM Wiki Plan

## Goal

Create a lightweight Karpathy-style LLM wiki in `KB/` for developing a real estate referral-fee business.

The core thesis:

A real estate agent can earn referral income by matching prospective buyers or sellers with trusted agents in another geography, typically collecting around 25% of the referred agent's commission. The goal is to discover repeatable campaigns and channels that can produce thousands of qualified referrals per year.

Example:

If a person in the San Francisco Bay Area wants to buy a home in San Luis Obispo, a Bay Area agent can refer that person to a San Luis Obispo agent and collect a referral fee from the receiving agent's commission.

The wiki should track:

- Business and channel ideas for generating referral fees
- Concrete campaign experiments
- Promising origin-destination market pairs
- Known or candidate referral partner agents
- Daily research outputs from recurring tasks

## Simple Directory Structure

```text
KB/
  SCHEMA.md
  index.md
  log.md

  ideas/
  campaigns/
  markets/
  agents/
  research/
  inbox/
```

## Folder Meanings

`ideas/`

Reusable business model, acquisition, or channel ideas for generating referral fees. Examples:

- Alumni relocation campaigns
- SEO pages for people moving between markets
- LLM discovery pages / `llms.txt`
- Pair-wise geography outreach
- Realtor partner networks

`campaigns/`

Specific experiments that could be run. Examples:

- Cal Poly alumni move-back-to-SLO campaign
- San Diego to Bay Area relocation campaign
- San Francisco to Maui campaign
- San Francisco to Tennessee campaign

`markets/`

Geographic pair or destination notes. A market page can describe why people might move, target audiences, likely demand, and known referral agents.

Examples:

- `sf-to-slo.md`
- `san-diego-to-bay-area.md`
- `sf-to-maui.md`
- `sf-to-tennessee.md`

`agents/`

Known or candidate referral partner agents by geography. Keep this practical:

- Name
- Market
- Specialty
- Referral terms
- Trust level
- Contact notes

`research/`

Daily cron outputs and summarized findings. This can include competitor scans, SEO ideas, LLM discovery research, market demand notes, alumni/community research, and compliance notes.

`inbox/`

Raw unprocessed notes, pasted ideas, URLs, transcripts, and cron dumps before they are cleaned up.

## Core Files

### `KB/SCHEMA.md`

This file tells future LLM agents how to maintain the wiki.

Initial contents:

```md
# KB Schema

This is a lightweight LLM-maintained wiki for building a real estate referral-fee business.

Core thesis:
A real estate agent can earn referral income by matching prospective buyers or sellers with trusted agents in another geography, typically collecting around 25% of the referred agent's commission. The goal is to discover repeatable campaigns and channels that can produce thousands of qualified referrals per year.

Folders:
- ideas/: reusable business/channel ideas
- campaigns/: concrete experiments to test
- markets/: geography-pair or destination notes
- agents/: known referral partner agents
- research/: research outputs and daily cron summaries
- inbox/: unprocessed notes and source material

Rules:
1. Keep pages short and useful.
2. Prefer updating existing pages over creating duplicates.
3. Every campaign should link to at least one market and one idea when possible.
4. Every market page should mention known agents if we have them.
5. Daily research should first go to research/, then only important conclusions should be promoted into ideas/, campaigns/, markets/, or agents/.
6. Update index.md whenever adding an important page.
7. Append every meaningful change to log.md.
```

### `KB/index.md`

Content map of the wiki.

Example:

```md
# KB Index

## Ideas
- [[ideas/llm-discovery-pages]] - Use LLM-readable pages so buyer agents and AI agents can discover referral partners.
- [[ideas/alumni-relocation-campaigns]] - Target alumni who may want to move back to college towns.

## Campaigns
- [[campaigns/cal-poly-alumni-slo]] - Ask Cal Poly alumni if they have thought about moving back to San Luis Obispo.

## Markets
- [[markets/sf-to-slo]] - Bay Area residents considering San Luis Obispo.
- [[markets/san-diego-to-bay-area]] - San Diego residents considering the Bay Area.
- [[markets/sf-to-maui]] - Bay Area residents considering Maui.
- [[markets/sf-to-tennessee]] - Bay Area residents considering Tennessee.

## Agents
- [[agents/san-luis-obispo]] - Referral partner candidates in San Luis Obispo.

## Research
- [[research/2026-05-25-daily-scan]] - Daily research summary.
```

### `KB/log.md`

Append-only timeline of meaningful wiki activity.

Example:

```md
# KB Log

## [2026-05-25] create | KB initialized
- Created lightweight schema for referral-fee business experiments.
```

## Page Templates

### Idea Page

```md
---
type: idea
status: seed
---

# Idea Name

## Thesis

## Why It Might Work

## Risks

## Related Campaigns

## Related Markets

## Next Step
```

### Campaign Page

```md
---
type: campaign
status: brainstorm
---

# Campaign Name

## Audience

## Pitch

## Target Market

## Referral Agent Needed

## Channel

## Success Metric

## Notes

## Next Step
```

### Market Page

```md
---
type: market
---

# Origin to Destination

## Why People Might Move

## Target Audiences

## Known Referral Agents

## Campaign Ideas

## Research Notes
```

### Agent Page

```md
---
type: agent
market:
status: candidate
---

# Agent / Market Name

## Geography

## Specialty

## Referral Terms

## Trust Notes

## Contact Notes
```

## Daily Research Loop

Start with one daily research output file:

```text
KB/research/YYYY-MM-DD-daily-scan.md
```

The recurring research task should answer:

1. What new referral-fee campaign ideas appeared today?
2. What market pairs seem promising?
3. What SEO or LLM-discovery opportunities exist?
4. What agent-partner gaps should we fill?
5. What existing KB pages should be updated?

The cron should not try to fully rewrite the wiki every day. It should:

1. Create a daily note in `KB/research/`.
2. Summarize the most useful findings.
3. Promote only important conclusions into `ideas/`, `campaigns/`, `markets/`, or `agents/`.
4. Update `KB/index.md` if important pages are added.
5. Append a short entry to `KB/log.md`.

## Initial Seed Pages

```text
KB/
  SCHEMA.md
  index.md
  log.md
  ideas/
    alumni-relocation-campaigns.md
    llm-discovery-pages.md
  campaigns/
    cal-poly-alumni-slo.md
  markets/
    sf-to-slo.md
    san-diego-to-bay-area.md
    sf-to-maui.md
    sf-to-tennessee.md
  agents/
    san-luis-obispo.md
  research/
  inbox/
```

## First Implementation Pass

1. Create the `KB/` directory structure.
2. Add `SCHEMA.md`, `index.md`, and `log.md`.
3. Seed the first idea, campaign, market, and agent pages.
4. Add a single daily research prompt or script.
5. Review the first few cron outputs manually before automating page updates.

The guiding question for the wiki:

What repeatable systems can create qualified real estate referrals at scale?
