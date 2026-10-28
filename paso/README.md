# Paso — client paperwork tracker

**Customer:** a family import/export business that ran all client paperwork through WhatsApp chats.
**My role:** product definition, spec, and building it end to end.
**Status:** live at [holapaso.com](https://holapaso.com) and used by the first customer.

## The problem

From the [PRD](PRD.md), three pains came up again and again:

1. Clients send photos (PNG/JPEG) when the business needs PDFs.
2. Staff tell clients about every status change by phone or text, by hand.
3. Documents and updates get lost in WhatsApp scrollback, and nothing shows where each job stands.

## The product

- Staff build **workflows**: stages that run in order, each holding steps that happen side by side, with forks where an answer decides the path.
- Each client job follows one workflow, so staff and client see the same map.
- Clients act from **one private link with no login**, usually opened from WhatsApp. Photos become PDFs automatically and land on the right step.
- The interface is in English and Spanish. It also has voice messages and a "Listen" button for clients who don't read well.

## Key product decision

The spec planned the **WhatsApp Business Cloud API**. I built it and tested it, but didn't ship it. Instead, staff send each client a private link from their own WhatsApp, with the message already written for them.

- **Why:** clients keep the number they already know. There is no Meta business verification, template approval or provider fee. Family chats on that number never touch the system. And there is no risk of a ban from unofficial relays.
- **Trade-off:** sending a notification takes one tap instead of zero, and photos dropped straight into the chat aren't captured automatically.
- **Way back:** the API code stays in the repo, so full automation is an upgrade later, not a rewrite.

## Documents

- [PRD](PRD.md): purpose, users, v1 scope and non-goals, architecture, data model, error handling, testing approach, open items
- [Product brief](product-brief.md): users, positioning, operating context and product principles
