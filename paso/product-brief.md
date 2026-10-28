# Product

## Platform

web

Installable on phones as a home-screen app (PWA); on Android it also appears in the share sheet so a photo or PDF can be shared straight into Paso.

## Users

- **Small service teams.** Owners and staff of small businesses that run client paperwork. The first customer is a family import/export business: the founders (mom and dad) and their staff. They work mostly on phones and talk to clients on WhatsApp. Their job is to know what each job still needs, collect it, check it, and move the job along without chasing people by phone.
- **Their clients.** They arrive from one private link sent on WhatsApp, with no account and no password. Many read Spanish first, and some don't read well. Their job is to see what is needed now, send it (usually a photo), and know where their job stands.

## Product Purpose

Paso replaces running client paperwork through WhatsApp chats alone. Each job follows a workflow, so nothing gets lost in chat scrollback. Clients send what is needed from their phone, staff check it, and both sides see where the job stands. Success means fewer reminders sent by hand and no lost documents. Clients finish their part without help, and staff can tell at a glance what is waiting on whom.

## Positioning

- **The workflow is one map.** Every job follows a workflow of stages that run in order, each holding steps that happen side by side, with forks where an answer decides the path. Staff and the client see that same map, so "where are we" has one answer.
- **WhatsApp and one link, no login.** Clients act from a single private link and from WhatsApp, the channel they already use. Photos become PDFs automatically and are filed to the right step.

## Operating Context

- **Workflows and jobs.** Staff build workflows (templates) and start a job for a client from one. Each job carries its own copy of the workflow, which can be changed for that job alone.
- **Steps.**
  - A step belongs to the client or to our team.
  - It takes files, an answer picked from a list, a short typed answer, or nothing (a check-off).
  - A step can also record amounts (cost, charge or payment) that add up in the job's money summary.
  - Our team approves or sends back what a client sent, with a reason the client reads.
- **Channels.** Clients get WhatsApp messages from a consenting staff member's WhatsApp Business number, plus reminders when a job goes quiet. Staff get push notifications on their phones.
- **Other tools.**
  - A shared inbox for files that arrive by email or share sheet.
  - Voice messages and a "Listen" button for clients who don't read.
  - Paso can be operated from Claude through its MCP connector, either headless or "bridge" mode, where the person's open Paso window follows along.
- **Where it is used.** Staff in Chile and Mexico, on phones first. Clients open their link from a WhatsApp chat.

## Capabilities and Constraints

- **Built.** Workflows, jobs, clients, team, inbox, client links, PDF conversion and multi-page merge, bilingual UI, WhatsApp notifications and reminders, push notifications, step amounts with a money summary, and the MCP connector.
- **Market-neutral.** The parents' import/export business is only the first customer. Paso must stay usable by any trade with client paperwork (accountants, notaries, contractors, agencies and so on). The product must have no industry-specific words, defaults or rules; labels, units and currencies are set by each business.
- **Bilingual.** English and Spanish throughout, with an explicit language setting for every person. Spanish addresses people as "usted".
- **Shown to clients.** A client's link only ever shows that client's own job and what they need to do. Costs and internal amounts are never shown to clients.
- **Not built.** Paso does not issue legal tax invoices (SII factura, CFDI). Money tracking produces totals and statements, and invoices stay in each business's accounting system.
- **Planned, not built.** More kinds of recorded value (dates, quantities, references), statements and exports built from amounts, and an "Agreement" step where the client accepts something.
- **Stack.** Next.js on Vercel (holapaso.com, deployed from `main`), Supabase for Postgres, auth and storage, and WhatsApp's Cloud API.

## Brand Commitments

- **Name.** The product is named **Paso**. Tagline: "Paperwork that moves step by step. Paso a paso."
- **Voice.** Plain, short sentences in everyday words, the way the business itself talks. The interface uses "job" in English and "gestión" in Spanish, not technical terms.
- **Trademark.** Clearance in the US and Mexico has been recommended but not done.

## Evidence on Hand

- [PRD.md](PRD.md) is the original product spec.
- The live product at holapaso.com is used by the first customer.
- There are no testimonials, customer counts, pricing or case studies. Do not invent any.

## Product Principles

1. **One concept, one place.** A value is edited where it is shown, never in a second field elsewhere. Simplicity, efficiency, elegance.
2. **Scannable at a glance.** Any card or row shows at most two pieces of information. Detail goes to tables or panels.
3. **Phone first, never panning sideways.** Every screen must work at 320px wide, for both staff and clients.
4. **The client never needs help.** Anything a client sees must be doable from a WhatsApp link, in their language, without reading being required.
5. **Steps drive outcomes.** A step's job is to produce something the business uses (a document, an answer, an amount), not only a record that it happened.

## Accessibility & Inclusion

- Many clients read poorly or not at all. Pages and messages can be listened to, in the client's language and in the voice of the staff member they know.
- The UI uses large touch targets and plain words, and pairs every status color with a word.
- Light and dark themes are both supported.
