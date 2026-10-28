# Production checklist

What stands between the hosted demo and real use. Items need either nothing, the client's answers (questions Q1–Q26 in `validation-call-script.md`), or accounts.

## Done

- [x] Sign-in: invite-only, Google or password, server-side sessions, roles (`src/lib/auth`, `src/lib/roles.ts`)
- [x] Login rate limiting: 5 failed attempts per account or 20 per IP pause sign-in for 15 minutes (`src/lib/auth/rate-limit.ts`)
- [x] Money stored exactly: amounts, quantities and rates are `numeric(18,6)` (migration `0005`)
- [x] Migrations run at deploy (production builds only), not on each cold start (`scripts/migrate.mjs`)
- [x] Security headers: no framing by other sites, no MIME sniffing, strict referrer, HTTPS only, share links never cached (`next.config.ts`)
- [x] Receipts and BL scans go to private Vercel Blob storage when `BLOB_READ_WRITE_TOKEN` is set, and are always served through a signed-in route (`src/lib/files.ts`)
- [x] Server errors logged as one structured line each (`src/instrumentation.ts`; Vercel → Logs → search `server_error`)
- [x] A rate source failing for over a day shows on Tipos de cambio
- [x] Calculation rules layered Ajustes → cliente → BL; closing a BL freezes its rules
- [x] Cobro summaries sent by WhatsApp or email as revocable read-only links
- [x] BL scanner (ported from Paso): photo → page found, flattened, whitened → one PDF; read for free from the PDF text layer or by OCR (Tesseract), fields by rules (container check digits, B/L number, dates, vessel); Claude when a key is set
- [x] Deleting a cost also deletes its receipt file
- [x] Tests: legacy parity, journey, rules, passwords, rate limit, roles, sharing, rate alerts (`npm test`)

## Needs the client's answers

- [ ] Business rules Q1, Q2, Q3, Q13, Q14, Q20: confirm each, then set it in Ajustes
- [ ] Missing screens (Q4) from a second AnyDesk session: Demurrage, Bolivia, Kardex
- [ ] Permissions per role (Q21)
- [ ] What "Facturar" must produce (Q22)
- [ ] Legacy data: copy of the FoxPro folder (Q9), then import open BLs or start from a cutoff date
- [ ] Parallel run: enter the same BLs in both systems for a month and compare every bill to the cent before switching

## Needs accounts (Q24)

- [ ] Vercel **Pro** plan: Hobby is for non-commercial use only
- [ ] Neon: move to a paid plan, or schedule a daily export, for backups and a longer restore window
- [ ] **Give preview deployments their own Neon branch.** Today previews share the production database; migrations are already limited to production builds, but previews still read and write production data
- [ ] Vercel Blob store connected to the project (sets `BLOB_READ_WRITE_TOKEN`)
- [ ] Google OAuth client (`GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`) for "Continuar con Google"
- [ ] Optional: Anthropic API key (`ANTHROPIC_API_KEY`). Without it, BLs are still read for free (scanner + PDF text + OCR + rules); with it, Claude also reads shipper, consignee and cargo, and the assistant (lower-right chat) is turned on. Usage is billed per conversation by Anthropic
- [ ] Banco Central de Chile API account (`BCCH_USER`, `BCCH_PASS`): mindicador.cl often times out
- [ ] Custom domain, then update `APP_URL` and the Google redirect URI

## Before the first real BL

- [ ] `SEED_DEMO=false` and `ADMIN_EMAIL` set; start from a clean database (no demo BLs, no demo team)
- [ ] Change `DEMO_PASSWORD` or remove the demo accounts
- [ ] Train the Ops Lead, the Port Agent and the Owner; decide who in the company is the admin
- [ ] Privacy notice and data owner (Q26)

## Environment variables

See `.env.example` for every variable and what it does.
