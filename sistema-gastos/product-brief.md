# Product

## Platform

web

## Users

- **Primary: the Ops Lead**, operations staff at a logistics company in Iquique, Chile. Works at a desk on a laptop every day: registers each shipment (Bill of Lading), its containers, the costs it accumulates, and every truck that leaves for Bolivia. When the design has to trade off, it serves the Ops Lead first.
- **The Port Agent** pays shipping lines and the port, and today reports those costs late on a spreadsheet ("CF1"). Uses a phone, outdoors at the port.
- **The Owner** bills each client from the billing summary of each departure and needs to see what is ready to bill and what is costing money.
- **The Customs Coordinator** (customs) adds dispatch costs. Staff in **Santa Cruz, Bolivia** also connect (their tasks are not yet confirmed).
- Later: clients (Bolivian importers) may get read-only access to their own shipments.

## Product Purpose

Replace "Sistema Gastos", a Visual FoxPro desktop program (built by an outside vendor) installed on one laptop and reached over AnyDesk, with a web app. For every BL it tracks **costo** (what port, shipping lines, warehouse and trucks charge the company) and **cobro** (what the company charges its client), and produces a cost summary and a billing summary per departure. Success: the same numbers as the legacy system to the cent, partial departures handled, costs entered where they happen, and less effort than the old program.

## Positioning

Built around the life of a shipment (arrives → port → warehouse → departures to Bolivia → billing), not around cost categories. Billing amounts are never typed: they come from rates and margins. Exchange rates are pulled automatically from their published sources.

## Operating Context

- Spanish (Chilean) terminology: BL, contenedor, bodega, bodegaje, descarga, carguío, despacho, flete, cobro, Mantención.
- Currencies: CLP, USD, UF, BOB. The monthly "dólar aduanero" and BOB reference rates appear on every BL.
- Goods sit in rented warehouses in the ZOFRI free zone; trucks go to Santa Cruz. Shipments are often dispatched partially.
- The team coordinates through WhatsApp and Dropbox; the Owner strongly prefers Dropbox.

## Capabilities and Constraints

- v0 exists (Next.js + Postgres) with a tested calculation engine that reproduces the legacy summaries (BL 127: 1,643.21 cost / 1,859.47 billing).
- Fractional events: partial departures per container, leftovers keep accruing storage, BL-level costs split across departures.
- Open business rules are settings until the client confirms them (validation-call-script.md, Q1–Q20).
- Demurrage, Bolivian taxes, Bolivian entry costs and the container log have unconfirmed fields.
- No real login yet (demo role switcher).

## Brand Commitments

- App name stays **"Sistema Gastos"** (the name staff already use) until a corporate identity exists.
- Visual reference chosen by the user: the Wise design language, with **red** replacing Wise's green accent.

## Evidence on Hand

- Legacy screens and printouts (private). Discovery findings: [discovery-findings.md](discovery-findings.md). PRD: [PRD.md](PRD.md).
- Demo data: BL 127 and BL 134 rebuilt from the legacy screens (some fields marked SUPUESTO).
- The user's schematic of the legacy flow (tldraw).
- No company logo, customer quotes or business statistics exist; none may be invented.

## Product Principles

1. Follow the shipment, not the cost categories: every screen answers "where is this BL and what is missing?"
2. Show what needs doing first; detail opens only when needed.
3. Never ask for a number the system can calculate (billing, storage, conversions, rates).
4. One way to add a cost: pick what it is, and the system knows where it belongs.
5. The legacy numbers are the contract: the new system must match them to the cent.

## Accessibility & Inclusion

WCAG 2.1 AA. Phone use outdoors at the port (strong sunlight, one hand): large tap targets and high contrast.
