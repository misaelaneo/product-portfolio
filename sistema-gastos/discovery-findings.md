# Sistema Gastos — Reverse-engineering findings

Source: recording of the AnyDesk demo with the Ops Lead, 2026-10-15 (32 min).
Transcript: `recordings/2026-10-15-transcript.md` (local only — not in the repo) · Legacy screenshots: private (they show client data)
Timestamps like `[06:30]` point into the recording and transcript.

---

## 1. How it is built

| | |
|---|---|
| **Technology** | **Visual FoxPro** (confirmed 3 ways) |
| **Developer** | The original vendor (a Chilean software house)  |
| **Runs on** | One host laptop; the Ops Lead connects to it with AnyDesk |
| **Integrations** | None — no connection to Aduana, SII or ZOFRI `[24:12–24:25]` |
| **Currencies** | CLP, USD, BOB (BOB only as a reference for land freight) `[27:33–28:30]` |

**Evidence for Visual FoxPro**
1. The Ops Lead says so at the start of the recording `[00:07]`.
2. The report window title is `Report Designer - resumcosto.frx` — `.frx` is a FoxPro report file.
3. Every window caption shows its internal FoxPro form name in brackets, e.g. `[bls]`, `[blsingreso1]`.

**What this means for recovering the logic**
- The program is an `.exe` compiled from FoxPro forms (`.scx`) and code (`.prg`). FoxPro executables can usually be **decompiled back to readable source** (e.g. with ReFox or a similar tool).
- FoxPro apps normally store data in **`.dbf` tables** in a folder next to the program. Those tables contain the full data model and history.
- **Check ownership first:** the original vendor wrote it. Ask the Owner what the licence/contract says before decompiling. The original vendor may also simply have the source.

**Internal form names seen**

| Form | Screen |
|---|---|
| `bls` | BL list + BL editor (main screen) |
| `blsingreso` | Create/update BL |
| `blsingreso1` | Containers of a BL |
| `blsingreso3` | Print billing summary — choose dispatch operation |
| `recepcion` | Port reception and transfer to warehouse |
| `bodega2` / `bodega2ing` | Warehouse storage list / edit |
| `bodega3` | Warehouse unloading |
| `bodega4` | Warehouse loading |
| `gdespach1` | Dispatch costs |
| `fletes` / `fleingreso` | Freight to Santa Cruz list / edit |
| `resumcosto.frx` | Cost summary report (title also shows this name for the billing summary) |

---

## 2. What the business does

A logistics company in **Iquique (ZOFRI free zone)** that imports goods on behalf of **clients in Bolivia** (e.g. Client A, Client B), stores them in rented warehouses, and ships them by truck to **Santa Cruz**. `[01:08–01:23]`

For every shipment (BL) it records **cost** (what suppliers charge the company) and **cobro** (what the company charges the client), and prints two summaries. The Owner bills clients from the billing summary. `[17:38–18:36]`

**People**
- **Owner**: bills clients.
- **Ops Lead**: enters data into the system.
- **Port Agent**: pays shipping lines/port and sends costs on a spreadsheet ("CF1"). `[08:06–08:15]`, `[20:24–20:49]`
- **Customs Coordinator**: notary, customs agency, weighing/sealing trucks, dispatch costs. `[16:37–16:42]`

---

## 3. Workflow and screens

Menu: **Ingreso** (clientes, proveedores, centro de costo, Bill of Lading) · **Informes** · **Mantención** `[00:12]`

1. **BL list** — all open BLs; "Mostrar todos (histórico)" shows closed ones. Buttons: Nuevo, Actualizar, Elimina, Contenedor, O.Despacho, Excel. `01-bls/01-lista-bls.png`
2. **Create/update BL** `[00:52–05:04]` — `01-bls/02-actualiza-bl.png`
3. **Containers** `[05:06–06:10]` — each container with unit, quantity, balance and agreed margin. `01-bls/03-contenedores-bl.png`
4. **Process BL** → editor with 8 cost sections + 2 reports. `01-bls/04-editor-bl-menu-gastos.png`
   - Recepción en puerto y traslado a bodega `[06:25–09:09]`
   - Gastos de bodega, descarga y carga `[09:10–15:35]`
   - Gastos despacho `[15:36–16:43]`
   - Demurrage *(not shown)*
   - Fletes Santa Cruz `[17:06–17:36]`
   - Impuestos pagados en Bolivia *(not shown)*
   - Gastos de ingreso en Bolivia *(not shown)*
   - Kardex movimiento contenedor – Almacén Iquique *(not shown)*
5. **Print Resumen Costo / Resumen Cobro** `[17:38–18:36]` — `04-reports/`

### BL fields
ID · Cliente · BL number · Detalle (containers + product) · Nro. ingreso mercadería (ZOFRI acceptance code) · Fecha · T.Cambio USD (monthly customs dollar) · T.Cambio BOB · Proveedor (foreign supplier) · Nave (vessel) · Carga (product type) · Valor CIF · BL terminado (closed flag)
**Per-BL rates:** Bodega almacén USD/día (costo, cobro) · Bodega descarga valor UF + UF costo/cobro · Parqueo rampla (costo, cobro) · Parqueo camión c/carro (costo, cobro)

### Containers
Contenedor · U.Medida (e.g. SACO) · Cant. ingreso · Saldo · Margen (USD per container, e.g. 1000)

---

## 4. Calculations (verified against the numbers on screen)

| # | Where | Formula | Check from screens |
|---|---|---|---|
| 1 | Recepción puerto | Total USD = Total CLP ÷ paridad dólar | 686,000 ÷ 925.25 = **741.42** ✓ |
| 2 | Almacenaje | Monto USD = m² × días × USD/día (rate from BL) | 11.84 × 31 × 0.23 = **84.42** cost; × 0.40 = **146.82** cobro ✓ |
| 3 | Almacenaje | Días = fecha final − fecha inicio **+ 1** (inclusive) | 15/09 → 15/10 shows **31** |
| 4 | Descarga / Carga | Tarifa grúa = valor UF × precio UF | 40,844.79 × 0.35 = **14,296** cost; × 0.50 = **20,422** cobro ✓ |
| 5 | Descarga / Carga | Total = horas grúa × tarifa + grúa externa × tarifa + apoyo persona × tarifa (+ parqueo in Carga) | 2 × 14,296 + 10,000 = **38,592** ✓ |
| 6 | Fletes Sta. Cruz | Monto BOB = cantidad camiones × valor pactado; USD = BOB ÷ paridad BOB | 2 × 34,000 = **68,000 BOB** → 6,800 USD ✓ |
| 7 | Reports | CLP sections converted to USD at paridad dólar | 132,861 ÷ 916.97 = **144.89** (carga cost) ✓; 191,230 ÷ 916.97 = **208.55** (carga cobro) ✓ |
| 8 | Reports | Total = sum of sections; BOB freight in its own column | Costo 206.44 + 144.89 + 1,291.88 = **1,643.21** ✓; Cobro 359.04 + 208.55 + 1,291.88 = **1,859.47** ✓ |

**Profit comes from**
- Cobro rates higher than cost rates (storage 0.40 vs 0.23 USD/día; UF 0.50 vs 0.35; parking 65,000 vs 45,000).
- **Margen per container** (e.g. USD 1,000) agreed with each client `[05:40–05:51]`.
- **Servicio de tramitación**: CLP 100,000 per container, entered as a port-reception line `[08:47–09:05]`.
- Gastos despacho are passed through at cost (same 1,291.88 in both reports).

---

## 5. Pain points and what they want

1. **Partial dispatches** are not supported. When only part of a shipment leaves, the remaining units and m² must stay on record and keep accruing storage until the next truck. `[25:39–27:19]`
2. **The Port Agent's spreadsheet (CF1)**: port and shipping-line costs arrive late, on a spreadsheet. The team wants the Port Agent to enter costs directly in the system as they happen, with receipts attached later. `[20:24–22:56]`
3. **Cloud access** — staff (the Port Agent, the Customs Coordinator, the Ops Lead) working at the same time, and possibly **clients viewing their own shipments** read-only. `[18:41–19:22]`, `[21:09–21:21]`
4. Data entry itself is not a problem. `[25:07–25:12]`
5. Today they coordinate through WhatsApp and Dropbox. **The Owner strongly prefers Dropbox** — don't plan on moving them to Google Drive. `[28:51–30:01]`
6. **People in Santa Cruz (Bolivia) also connect to the system** — so users are in two countries. `[30:06–30:11]`

---

## Agreed next steps (end of call)
- Present a **corporate identity + redesign proposal** to the Ops Lead and the Owner, then build a **prototype** and review the app architecture. `[30:20–31:09]`
- Meeting proposed for **Monday 2026-10-19**. The Ops Lead will mention it to the Owner; we will also message the Owner. `[31:25–31:50]`

---

## 6. Open questions

- [ ] **Carguío parking:** shows Días = 4 and Precio = 45,000 but Total = 45,000. Are days ignored, or is the price already for the whole stay? (`03-bodega/05-carguio-bodega.png`)
- [ ] **"Margen pactado"** appears in the billing report but is blank for BL 127, even though its containers have a margin. When does it get filled in?
- [ ] **Gastos despacho** shows "Total $" = "Total USD" = 1,291.88. Bug, or are these entered in USD only?
- [ ] The billing summary window is also titled `resumcosto.frx`. One report with a switch, or a mislabelled second report?
- [ ] The Fletes screen labels the BOB rate "Paridad Dólar". Just a mislabel?
- [ ] Screens not shown yet: Demurrage, Impuestos Bolivia, Gastos ingreso Bolivia, Kardex contenedor, O.Despacho, Informes menu, Mantención menu, Clientes/Proveedores/Centro de costo.
- [ ] **Installation files / source:** the Ops Lead will ask the Owner how it was installed and whether there is a backup `[23:16–24:04]`. Also ask whether the original vendor can provide the source code.
- [ ] Get a copy of the program folder and its `.dbf` data (with the program closed). Run [tools/app-inventory.ps1](../tools/app-inventory.ps1) on the host laptop.

---

## Notes on the transcript
- Machine-generated (faster-whisper large-v3). Names are sometimes misheard: "Vilos Vlade" = Bill of Lading, "Sofri" = ZOFRI, "Japa del Doy" = Hapag-Lloyd, "Ikea" = Iquique.
- `[16:44–17:04]` is a transcription glitch (a repeated "¿De cuánto?"); the audio there is the Ops Lead scrolling through the dispatch screen.
