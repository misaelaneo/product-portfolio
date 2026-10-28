# Gap-closing call — v0 walkthrough script

**Goal:** show the v0 prototype to the Owner and the Ops Lead and, at each screen, close the open questions from the [PRD](PRD.md#14-open-questions).
**Who:** the Owner (decisions), the Ops Lead (daily use). The Port Agent and the Customs Coordinator are a bonus for steps 4 and 7.
**Time:** about 45 minutes.

**How to use this document**
- Follow the steps in order; each one is a screen in the app.
- At each step: show the screen → say what v0 does today ("**v0 assumes**") → read the question aloud (Spanish, in quotes) → write the answer in the box.
- Most answers are a **setting change** in *Ajustes → Reglas por confirmar*, not new code. The "**Goes to**" line says where each answer lands.

---

## Before the call (10 min)

- [ ] Open the app (`npm run dev` → http://localhost:3000, or the deployed URL).
- [ ] *Ajustes → Restablecer datos de demostración*, so BL 127 and BL 134 start clean.
- [ ] *Tipos de cambio → Actualizar ahora* (the Bolivian official rate comes in; UF / dólar may fail if mindicador.cl is down — that's a good talking point).
- [ ] Have the legacy screenshots ready: (private).
- [ ] Have this document open to take notes.

**Opening line:** *"Esto es una primera versión para validar que entendimos bien su sistema. Los números de los BL 127 y 134 son los mismos del sistema actual. Donde tuvimos que suponer algo, la pantalla lo indica como 'Regla por confirmar', y hoy queremos confirmarlos con ustedes."*

---

## 1. Pendientes (home: what needs doing today)

**Show:** goods in the warehouse, storage accruing, exchange rates, pending receipts.

**Q12 — Volume**
> "¿Cuántos BL trabajan al mes, aproximadamente? ¿Y cuántas personas usarían el sistema?"

Goes to: hosting size and pricing.
Answer: ____________________________________________

**Q6 — Santa Cruz office**
> "Nos mencionaron que en Santa Cruz también se conectan al sistema. ¿Qué hacen ahí: solo consultan, o también ingresan datos? ¿Cuáles?"

Goes to: roles (PRD §4) and possibly a Santa Cruz module.
Answer: ____________________________________________

---

## 2. BLs → Nuevo BL (rates filled in automatically)

**Show:** open *Nuevo BL*. The dollar, UF and boliviano fields fill themselves, and each one shows its source and date. Change one value: it becomes "ingreso manual".

**v0 assumes:** dollar = monthly customs dollar, falling back to dólar observado; boliviano = BCB official rate; UF = latest available.

**Q15 — Customs dollar**
> "El dólar que usan en cada BL (por ejemplo 925,25 en septiembre), ¿de dónde lo sacan exactamente? ¿Quién lo publica y qué día del mes?"

Goes to: an automatic source for `USD_ADUANERO` (today it's typed in monthly).
Answer: ____________________________________________

**Q16 — Boliviano rate**
> "Para el boliviano usaron 10 y 11 en los BL de julio y septiembre. ¿Es el oficial del Banco Central de Bolivia, el del mercado, o el que acuerdan con el transportista? ¿Es el mismo para costo y para cobro?"

Goes to: default boliviano rate type for BLs and freight.
Answer: ____________________________________________

**Q17 — Which UF date**
> "Para la tarifa de grúa usan el valor UF. ¿De qué fecha: la del BL, la del día de la descarga o la del despacho?"

Goes to: UF lookup date.
Answer: ____________________________________________

**Q19 — BL number in the list (new)**
> "En la lista de BLs del sistema actual, la columna 'Nro.' no coincide con el ID del BL (por ejemplo, Nro. 120 corresponde al ID 134). ¿Qué es ese número? ¿Lo usan para algo?"

Goes to: whether the new list needs a second number.
Answer: ____________________________________________

---

## 3. BL 134 → Contenedores

**Show:** 5 containers × 20 sacks, 11.84 m² each, USD 1,000 margin each.

**v0 assumes:** m² is stored per container. After a partial dispatch, the m² still in the warehouse is proportional to the units left.

**Q13a — m² of what stays**
> "Si de un contenedor de 20 sacos salen 18, ¿los 2 que quedan ocupan la parte proporcional del espacio (un décimo), o se vuelve a medir el espacio que ocupan?"

Goes to: m² rule (proportional vs. re-measured per container).
Answer: ____________________________________________

**Margin per container**
> "¿El margen de 1.000 dólares es siempre por contenedor, o puede ser por saco, por kilo o un monto fijo por BL?"

Answer: ____________________________________________

---

## 4. Step 2 · Puerto + phone module

**Show:** BL 134 → step 2 *Puerto* (5 lines, same as the legacy screen) and the single *Agregar gasto* button. Then open **Celular** on a phone-size window, switch the role to **Port Agent** there, and use **Agregar costo de puerto**. Save a cost without a photo: it appears in the BL as "receipt pending".

**v0 assumes:** port costs are passed through to the client at cost (they appear the same in cost and billing).

**Q20 — Port costs in billing (new)**
> "Los costos de puerto (flete collect, gate in, aforo, traslado), ¿se le cobran al cliente al mismo valor? ¿O hay alguno que no se cobra, o que lleva recargo?"

Goes to: *Ajustes → Cobrar los costos de puerto al cliente al mismo valor*.
Answer: ____________________________________________

**Q10 — CF1 spreadsheet**
> "¿Nos pueden enviar un ejemplo de la planilla CF1 que manda the Port Agent? Así el formulario del celular pide exactamente lo mismo."

Answer / sent?: ____________________________________________

**Q18 — Phones and signal**
> "¿the Port Agent y la gente de bodega tienen buena señal en el puerto y en la bodega? ¿Usan Android o iPhone?"

Goes to: whether offline entry is needed (PRD MOB-9).
Answer: ____________________________________________

---

## 5. Bodegaje (storage)

**Show:** BL 134 → step 3 *Bodega*: the lines compute themselves from the arrival date, 11.84 m² × days × 0.23 / 0.40.

**Confirm the day count:**
> "Del 15/09 al 15/10 el sistema actual cuenta 31 días, o sea, cuenta el primer y el último día. ¿Siempre es así?"

Answer: ____________________________________________

**Q13b — Storage after a partial dispatch.** Show *Ajustes → Bodegaje cuando un contenedor sale en partes* and explain both options with the example:

> "Ejemplo: llegan 20 sacos el 15/09. El 30/10 salen 18 y el 20/11 salen los 2 últimos.
> Opción A (por unidad): cada saco paga desde que llegó hasta que salió. El despacho del 30/10 paga 18 sacos por 46 días; el del 20/11 paga 2 sacos por 67 días.
> Opción B (por período): el despacho del 30/10 paga todo el espacio ocupado hasta ese día, y el del 20/11 paga solo lo que quedó después.
> El total cobrado es el mismo; cambia a qué despacho se le carga. ¿Cuál corresponde a cómo cobran ustedes?"

Goes to: *Ajustes* setting (v0 default: **por unidad**, which matches the legacy BL 127 printout).
Answer: ____________________________________________

---

## 6. Descarga and Carga

**Show:** BL 134 *Descarga* (forklift rate = UF × 0.35 / 0.50; totals 164,368 / 263,376, same as legacy). BL 127 *Carga*.

**Q1 — Parking days**
> "En el carguío del BL 127, el parqueo dice 4 días a 45.000, pero el total es 45.000, no 180.000. ¿El precio del parqueo es por día o por toda la estadía?"

Goes to: *Ajustes → Multiplicar el parqueo por los días* (v0 copies legacy: days are not multiplied).
Answer: ____________________________________________

**External forklift and support staff**
> "La grúa externa aparece en 0 en los BL que vimos. ¿Cuánto cuesta y cobran cuando se usa? ¿Y el apoyo de personas, siempre es 10.000 de costo y 20.000 de cobro? ¿Se cobra por persona por cada descarga o carguío, o por persona por hora?"

Goes to: today the app (like legacy) charges personas × tarifa, once per descarga or carguío. If it is per hour, we add *horas de apoyo* and multiply.

Answer: ____________________________________________

---

## 7. Step 4 · Salidas — live partial departure (the main new feature)

**Show:** BL 134 → *Registrar salida*. Tap *Todo* on three containers, type 18 on UACU387717-7 (the screen counts "Salen 78 de 100 sacos"), and register it. Step 5 *Facturar* then shows the departure ready to bill, with *Marcar facturada*. Then show:
- the BL is now "Parcialmente despachado", with 22 of 100 sacks in the warehouse
- UACU387717-7 keeps 2 sacks on 1.18 m², still accruing storage
- the dispatch's billing summary: 78 % of port and unloading costs, 3.9 containers of margin

**Q14 — How shared costs are split**
> "Cuando un BL sale en varios despachos, los costos que son de todo el BL (puerto, descarga, margen) ¿cómo se reparten? ¿Por cantidad de sacos, por metros cuadrados, por valor CIF, o todo en el primer despacho?"

Goes to: *Ajustes → Cómo se reparten los costos de todo el BL entre salidas* (v0 default: **by units**).
Answer: ____________________________________________

**Q2 — Agreed margin on the bill**
> "En el resumen de cobro del BL 127, 'Margen pactado' salía en blanco aunque los contenedores tienen margen. ¿Cuándo se llena? ¿Se cobra en el primer despacho, en el último, o repartido?"

Answer: ____________________________________________

**Q3 — Dispatch costs currency**
> "En gastos de despacho, 'Total $' y 'Total USD' muestran el mismo número (1.291,88). ¿Esos gastos se ingresan siempre en dólares?"

Answer: ____________________________________________

---

## 8. Demurrage and Bolivia

**Show:** *Agregar gasto* → Demurrage / Impuestos en Bolivia / Gastos de ingreso en Bolivia. Explain that these are generic lines for now because we didn't see these screens.

**Q4 — Missing screens**
> "Estas pantallas no las vimos: Demurrage, Impuestos pagados en Bolivia, Gastos de ingreso en Bolivia y el Kardex de contenedores. ¿Podemos hacer una sesión corta de AnyDesk para verlas? Si no, ¿qué datos se anotan en cada una?"

Book: date / time ____________________
Notes: ____________________________________________

---

## 9. Summaries (reports)

**Show:** BL 127 → dispatch 102 → *Resumen costo* and *Resumen cobro*, next to the legacy printouts. Same lines, same totals: **1,643.21** and **1,859.47**.

**Q5 — Other reports**
> "Además de estos dos resúmenes, ¿qué otros informes usan del menú 'Informes'? ¿Y qué hay en 'Mantención'?"

Answer: ____________________________________________

**Q11 — What clients see**
> "Si más adelante los clientes pueden entrar a ver sus BL, ¿deberían ver solo el cobro, o también los costos?"

Answer: ____________________________________________

---

## 10. Tipos de cambio

**Show:** today's values per source, the comparison box, out-of-date flags, the log of automatic pulls (including any failed source), and manual entry with a reason.

> "Los valores se traen solos una vez al día. Si una fuente falla, el sistema avisa y se puede ingresar a mano con un motivo. ¿Hay alguna otra fuente que ustedes consulten y que debamos agregar?"

Answer: ____________________________________________

**Banco Central de Chile account**
> "Para traer la UF y el dólar directo del Banco Central de Chile se necesita una cuenta gratuita. Conviene que esté a nombre de la empresa. ¿Quién la puede crear?"

Who: ____________________

---

## 11. Access, ownership and data

**Show:** *Equipo* (people, roles, invite links) and *Ajustes → Registro de cambios*.

**Q21 — Who can do what**
Today: only admins manage Equipo and the global rules; admins and operators manage clients and client/BL rules; every role can register BLs, costs and departures. Proposed: Puerto only port costs and receipts; Aduana and Bodega read everything, Bodega also registers departures; only admins and operators delete.
> "¿Estos roles están bien? ¿Quién debe poder ver, modificar y borrar qué? Por ejemplo, ¿the Port Agent solo ingresa costos de puerto?"

Answer: ____________________________________________

**Q22 — What "Facturar" must produce**
Today step 5 only marks a departure as billed and the cobro summary can be sent by WhatsApp or email.
> "¿Qué documento emiten al cobrar: factura electrónica en el SII, factura de exportación, solo el resumen? ¿Desde qué sistema la emiten hoy?"

Answer: ____________________________________________

**Q23 — Carriers and electronic BLs**
> "¿Qué navieras emiten la mayoría de sus BL (Hapag-Lloyd, Maersk, EUKOR…)? ¿Reciben BL electrónicos hoy, por qué plataforma? ¿Quién recibe el BL: el importador, el agente o ustedes?"

Answer: ____________________________________________

**Q24 — Accounts in the company's name**
The live demo runs on accounts set up for the prototype. For production they should belong to the company (or be covered by contract).
> "¿A nombre de quién quieren las cuentas: Vercel (plan Pro, pagado), base de datos Neon, Google (para entrar con Google), Anthropic (lectura de BL), Banco Central de Chile (tipos de cambio) y el dominio (p. ej. gastos.suempresa.cl)?"

Answer: ____________________________________________

**Q25 — Sending summaries to clients**
> "¿Los clientes prefieren WhatsApp o correo? ¿Quién envía los resúmenes de cobro? ¿Hay un formato o membrete que deba llevar?"

Answer: ____________________________________________

**Q26 — Personal data**
Chile's data protection law (Ley 21.719) takes effect around December 2026 and covers client contacts and team accounts.
> "¿Tienen asesoría legal o contable para la ley de datos personales? ¿Quién en la empresa sería el responsable de los datos?"

Answer: ____________________________________________

**Q7 — Code ownership**
> "El sistema actual lo hizo the original vendor. ¿El código es de ustedes? ¿Tienen contrato o licencia? ¿the original vendor les podría entregar el código fuente?"

Answer: ____________________________________________

**Q8 — Installation and backups**
> "¿Cómo se instaló el sistema en ese laptop? ¿Hay respaldos de los datos? ¿Cada cuánto?"

Answer: ____________________________________________

**Q9 — Copy of the data**
> "Para migrar la historia necesitamos una copia de la carpeta del programa y de sus datos, sacada con el programa cerrado. ¿Cuándo la podemos sacar?"

When / how: ____________________________________________

---

## Close (2 min)

> "Con estas respuestas ajustamos la configuración y completamos las pantallas que faltan. Lo próximo es: (1) la sesión de AnyDesk para las pantallas que no vimos, (2) la copia de los datos para migrar, (3) la planilla CF1 de ejemplo. ¿Les parece?"

**Still to receive from the client**
- [ ] Second AnyDesk session: Demurrage, Bolivia screens, Kardex, Informes, Mantención (Q4, Q5)
- [ ] Copy of the program folder + `.dbf` data (Q9)
- [ ] Sample CF1 spreadsheet (Q10)
- [ ] Banco Central de Chile API account in the company's name
- [ ] Licence/contract with the original vendor, if any (Q7)
- [ ] Who owns each production account (Q24) and the domain to use
- [ ] A sample invoice they issue today (Q22)

---

## Quick reference — every open question, where it lands

| Q | Topic | v0 default | Where the answer goes |
|---|---|---|---|
| Q1 | Parking: per day or per stay | Not multiplied by days (legacy) | Ajustes setting |
| Q2 | When "Margen pactado" is billed | Margin × share of each container dispatched | Report rule |
| Q3 | Dispatch costs currency | USD | Dispatch cost form |
| Q4 | Demurrage / Bolivia / Kardex fields | Generic lines | New screens |
| Q5 | Informes and Mantención menus | Not built | Backlog |
| Q6 | Santa Cruz office tasks | Not modelled | Roles / module |
| Q7 | Code ownership (the original vendor) | — | Legal / decompiling |
| Q8 | Installation and backups | — | Migration plan |
| Q9 | Copy of legacy data | Demo data only | Migration |
| Q10 | CF1 spreadsheet | Generic port-cost form | Mobile form fields |
| Q11 | What clients see | — | Client portal (phase 2) |
| Q12 | Volume (BLs, users) | — | Hosting size |
| Q13 | m² and storage after partial dispatch | Proportional m²; storage **per unit** | Ajustes setting |
| Q14 | Splitting BL-level costs across dispatches | **By units** | Ajustes setting |
| Q15 | Customs dollar source | Typed in monthly | Rate source |
| Q16 | Boliviano rate type | BCB official | Rate default |
| Q17 | UF date | Latest value at BL creation | Rate lookup |
| Q18 | Phones and signal | Online only | Offline mode (MOB-9) |
| Q19 | "Nro." column in the legacy list | Not shown | BL list |
| Q20 | Port costs in billing | Passed through at cost | Ajustes setting |
| Q21 | Permissions per role | Admin: everything; operador: clients and rules; rest: BL work | `src/lib/roles.ts` |
| Q22 | What "Facturar" produces | Mark as billed + send cobro summary | Invoicing integration |
| Q23 | Carriers and eBLs | Scan/photo read on Nuevo BL | Carrier or eBL integration |
| Q24 | Accounts in the company's name | Prototype accounts | Hosting handover |
| Q25 | WhatsApp or email for summaries | Both available | Default channel, letterhead |
| Q26 | Personal data (Ley 21.719) | — | Privacy notice, data owner |

## Decisions log

| Date | Q | Decision | Decided by |
|---|---|---|---|
| | | | |
