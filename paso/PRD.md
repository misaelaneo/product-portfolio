# Mom & Dad Import/Export Deliverable Tracker — Design

## Purpose

A web app that lets the founders (mom & dad) and their staff manage and communicate deliverables for their import/export business, and lets clients submit documents and track progress — replacing the current WhatsApp-only, ad hoc process. It directly addresses three recurring pain points in the current process:

1. Clients submit documents as PNG/JPEG when the business's document trail requires PDF.
2. Clients must be notified of status changes manually (phone/text), which is time-consuming.
3. Documents and status updates get lost in WhatsApp chat scrollback with no structured view.

## Users

- **Admins** — mom, dad, and any other staff. Manage clients, build/edit workflow templates, act on deliverable steps, triage inbound messages.
- **Clients** — the business's customers. Submit documents, track deliverable status. Accounts are created only via admin invite (no self-registration). Clients may interact via the web portal, WhatsApp, or both — WhatsApp filing works even before a client ever logs into the portal.

## Scope

**In scope (v1):**
- Admin-configurable workflow templates (stages of parallel steps, run sequentially)
- Deliverable tracking against a live instance of a template
- Document upload (web + WhatsApp) with automatic PNG/JPEG → PDF conversion and admin-driven multi-page PDF merge
- WhatsApp Business integration (Meta Cloud API) for both inbound document/message submission and outbound status notifications
- Email + in-app notifications
- English/Spanish bilingual UI and WhatsApp templates
- Admin triage inbox for unmatched inbound WhatsApp messages

**Out of scope (v1):**
- Payments/invoicing (tracked elsewhere, e.g. bank/QuickBooks)
- Migrating the parents' existing personal/business WhatsApp number (v1 uses a new dedicated number; migration is a future decision once the system is proven)
- Fully arbitrary (branching/DAG) workflow graphs — v1 supports ordered stages of parallel steps, which covers the "mostly linear/parallel" processes described

## Architecture & Tech Stack

- **Framework:** Next.js (TypeScript, App Router) — single codebase serving the admin dashboard, client portal, and backend API routes.
- **Database, Auth, Storage:** Supabase (Postgres + Auth + Storage). Row-level security enforces that a client's session can only read/write their own Client's data; admins have role-scoped broader access.
- **Hosting:** Vercel, Hobby tier for v1. Note: Vercel's Hobby terms are non-commercial-use only, and this app supports a for-profit business, so this is a knowing, accepted risk — upgrade to Vercel Pro (~$20/mo) if usage grows or the account is ever flagged.
- **WhatsApp:** Meta WhatsApp Cloud API, direct integration (no BSP middleman), using a new dedicated business phone number for v1.
- **Email:** a transactional email provider with a free tier (e.g. Resend or Postmark).
- **File conversion:** server-side routine, triggered on upload from either source (web or WhatsApp), converts PNG/JPEG to PDF; a separate admin action merges multiple documents into one ordered multi-page PDF.
- **i18n:** English/Spanish toggle via a standard Next.js i18n library; every Client and admin User has an explicit preferred-language setting (no auto-detection) driving both UI copy and WhatsApp template selection.

Cost expectation: effectively free during build/testing; realistically ~$20–45/month once used for live business operations (Vercel Pro + eventual Supabase Pro for backups), plus Meta's per-conversation WhatsApp rates.

## Data Model

- **Client** — a customer account. Has one or more linked `PhoneNumber`s (many-to-one), email, preferred language, invite status.
- **PhoneNumber** — a phone number linked to exactly one Client. Used to match inbound WhatsApp messages to a Client. Admin-managed (added at invite time or during triage).
- **Deliverable** — the central tracked object. Belongs to a Client, references one `WorkflowTemplate` (as it existed at instantiation time), has a status derived from current stage/step progress.
- **WorkflowTemplate** — admin-authored, reusable across Deliverables (e.g. "Freight Forwarding — Mexico Import"). Composed of ordered `Stage`s. Editing a template only affects Deliverables created after the edit; in-flight Deliverables retain the Stage/Step structure they were started with.
- **Stage** — belongs to a WorkflowTemplate, has an order number. Contains one or more `Step`s that all run in parallel; the next Stage only activates once all of the current Stage's Steps are complete.
- **Step** — belongs to a Stage. Configurable: required document(s), assigned owner (admin or client), notify-client-on-change flag, requires-admin-approval flag. On a live Deliverable, each Step instance has its own status (pending/submitted/approved/rejected).
- **Document** — an uploaded file, linked to exactly one Step on one Deliverable. Always normalized to PDF for workflow purposes; original file retained for audit. May originate from the web portal or WhatsApp.
- **Message** — a log entry of an inbound or outbound WhatsApp/email communication, linked to a Deliverable for a full communication trail per shipment.
- **User (admin)** — mom, dad, staff. Role-based access; v1 treats all admins as equally privileged (create/edit templates, manage clients, act on steps, triage inbox).

## Workflow Engine

- **Authoring/editing:** admins build and edit WorkflowTemplates through a visual diagram view — Stages as ordered nodes containing their Steps, connected to show sequence — rather than a disconnected multi-step form. The whole workflow structure stays visible while editing. Clicking a Step opens a minimal-fields editor (required docs, owner, notify flag, approval flag) scoped to that step, without leaving the diagram context. (Detailed interaction/visual design for this diagram builder is deferred to a dedicated UX design pass using the `impeccable` skill, after this spec is approved.)
- **Instantiating a Deliverable:** admin picks a Client and a WorkflowTemplate; the template's Stage/Step structure is copied onto the new Deliverable as a live checklist. The first Stage's Steps start active; all others start pending.
- **Progression:** a Step completes when its requirements are met (document uploaded, and approved if required). A Stage completes when all its Steps are complete, which activates the next Stage. Steps within an active Stage can be completed in any order.
- **Client visibility:** clients see a simplified checklist/timeline of their Deliverable — completed stages, the active stage's steps (what's needed now), and locked/future stages grayed out. The diagram editor itself is an admin-only authoring tool.

## Document Handling

- Documents can be submitted via the web portal (drag/drop or mobile camera) or via WhatsApp (photo/PDF attachment); both paths converge on the same backend logic.
- Any PNG/JPEG upload is automatically converted to a single-page PDF on arrival. The original file is retained for audit, but the PDF is the canonical document for workflow purposes.
- Admins can select multiple documents already attached to a Step and merge them into a single ordered multi-page PDF (e.g. several photos of a multi-page customs form), replacing the individual pages as the Step's canonical document.
- Every Document is linked to exactly one Step on one Deliverable, determined either by which Step the client uploaded against (web portal) or by the WhatsApp routing logic below.
- Storage: Supabase Storage, private buckets, access gated by the same row-level security as the rest of the client's data.

## WhatsApp Integration

**Inbound (client → app):**
1. Message/media arrives at the dedicated WhatsApp number via Meta's webhook.
2. The sender's phone number is looked up against the PhoneNumber→Client mapping.
   - **Known number, exactly one active Deliverable:** auto-filed against the active Step (or, if the active Stage has multiple candidate Steps, a quick-reply asks which one).
   - **Known number, multiple active Deliverables:** an interactive list message asks the client which Deliverable this is for, then files accordingly.
   - **Unknown number:** the message/media lands in the admin triage inbox, unattached, until an admin links the number to a Client (existing or newly created).
3. Any image is normalized to PDF per the Document Handling rules above the moment it's filed.
4. If routing can't be resolved (e.g. client doesn't respond to a disambiguation prompt within a reasonable timeout), the message falls through to the triage inbox rather than being dropped.

**Outbound (app → client):**
- Step-level events (step started, document needed, approved/rejected, deliverable complete) trigger a WhatsApp notification using a pre-approved Meta message template, in the client's preferred language.
- Meta requires approved templates for business-initiated messages outside a 24-hour conversation window; the initial template set (a handful of status messages, in English and Spanish) must be submitted for Meta approval as part of setup, before go-live.
- Every inbound/outbound WhatsApp message is logged as a Message record tied to the relevant Deliverable.

**Admin triage inbox:** a dedicated view listing unmatched incoming messages, letting an admin link the sender's number to a Client (creating one if needed) and file the pending message against the right Deliverable/Step in one motion.

**Number strategy:** v1 uses a new, dedicated WhatsApp Business number, separate from the parents' existing personal/business number. This avoids putting their only current client-communication channel at risk during rollout of an unproven system (registering a number with the WhatsApp Business API removes it from the regular WhatsApp consumer app). Migrating the original number is a possible future step once the system is proven, not part of this design.

## Auth, Roles & Language

- **Admin accounts:** created directly for mom, dad, and staff; v1 treats all admins as equally privileged.
- **Client accounts:** created only via admin invite — admin enters name, phone number(s), email, and preferred language. Clients can submit via WhatsApp as soon as their number is linked, even before ever logging into the web portal.
- **Row-level security:** enforced in Postgres — a client's session can only query their own Client's Deliverables/Documents/Messages; admin access is role-scoped.
- **Language:** every Client and admin User has an explicit preferred language (English/Spanish), driving both web UI and WhatsApp template selection. Changeable at any time.

## Error Handling

- **Failed PDF conversion:** the Document is stored as-is (original format) and flagged for admin review, rather than silently dropped or blocking the Step.
- **WhatsApp delivery/template failure:** logged against the Message record; surfaced in the admin dashboard so staff can follow up manually.
- **Unresolvable inbound routing:** falls through to the triage inbox after a timeout, same handling as an unknown number.

## Testing Approach

Given solo maintenance and an MVP-speed priority, automated testing focuses on logic with real correctness risk and no visual feedback loop:
- Workflow progression logic (stage/step completion rules)
- Phone-number-to-Deliverable matching and disambiguation logic
- PDF conversion and multi-page merge

The diagram-based workflow builder and other UI get manual verification during development, plus a dedicated UX design pass (using the `impeccable` skill) after this spec, rather than heavy automated UI test coverage — prioritizing MVP velocity over UI test coverage at this scale.

## Open Items / Future Phases

- Migrating the parents' existing WhatsApp number once the new system is proven
- Possible admin role distinctions (e.g. owner vs. staff permissions) if needed later
- Supabase Pro upgrade (backups) once real client documents are at stake
- Detailed visual/UX design of the workflow diagram builder (next step, via `impeccable`)
