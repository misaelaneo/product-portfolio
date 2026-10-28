# Product Portfolio — Misa Escalera

Product work from discovery to shipped software. For each project I talked to the users, wrote the spec, built a working version and checked it against what they told me.

The source code of client projects is private. This repo holds the **product documents**: PRDs, discovery findings, interview guides, product briefs and launch checklists. Names of client staff and their customers are replaced with role names.

## Projects

| Project | What it is | PM documents | Status |
|---|---|---|---|
| [**Sistema Gastos**](sistema-gastos/) | Replacing a legacy desktop (Visual FoxPro) cost and billing system for a logistics company (Chile → Bolivia) with a web app plus mobile modules | [PRD](sistema-gastos/PRD.md) · [Discovery findings](sistema-gastos/discovery-findings.md) · [Validation call script](sistema-gastos/validation-call-script.md) · [Product brief](sistema-gastos/product-brief.md) · [Launch checklist](sistema-gastos/production-checklist.md) | v0 live with the client, in validation |
| [**Paso**](paso/) | Client paperwork tracker that moves a family import/export business off WhatsApp-only chaos | [PRD](paso/PRD.md) · [Product brief](paso/product-brief.md) | Live at [holapaso.com](https://holapaso.com), used by the first customer |
| [**Tabline**](tabline/) | Live League of Legends scoreboard over Twitch streams (extension, Windows app, backend, website) | [Product brief](tabline/product-brief.md) · [Website requirements](tabline/website-requirements.md) · [Riot API key application](tabline/riot-production-key-application.md) | In Twitch Hosted Test; Riot production key in review |
| [**Personal portfolio**](https://github.com/misaelaneo/misaescalera) | My portfolio site | [PRD](https://github.com/misaelaneo/misaescalera/blob/main/Documentation/PRDs/personal_portfolio_prd.md) | Live at [misaescalera.com](https://misaescalera.com) |
| [**Ghosty Bingo**](ghosty-bingo/) | Real-time bingo and birthday-card wall for a friend's birthday game night | [Product brief](ghosty-bingo/product-brief.md) | Shipped for the event (Sep 2026) |

## Where to start

Short on time? Read the **Sistema Gastos [PRD](sistema-gastos/PRD.md)**. It shows the whole process in one document:

- problems, each backed by a timestamp from the recorded discovery call
- every claim marked **[Confirmed]** or **[Assumed]**
- requirements ranked by priority
- open questions that become settings, so the client can answer them during a demo
- success measures and a phased plan

## How I work

1. **Discover.** Record a working session with the people who do the job, and pull out the problems, the workflow and the edge cases ([example](sistema-gastos/discovery-findings.md)).
2. **Specify.** Write a PRD that separates what was confirmed from what was assumed, with scope, non-goals and success measures ([example](sistema-gastos/PRD.md)).
3. **Build a v0 to learn.** Ship something real early. Every unconfirmed rule is a setting, not hard-coded.
4. **Validate.** Present the v0 and close the open questions screen by screen ([example](sistema-gastos/validation-call-script.md)).
5. **Make decisions and write them down.** For example, Paso dropped the WhatsApp Business API the spec planned and used private links instead ([why](paso/README.md#key-product-decision)).
