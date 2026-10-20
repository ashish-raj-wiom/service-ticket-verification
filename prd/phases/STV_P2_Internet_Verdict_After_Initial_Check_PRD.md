# Service Task Verification — P2: Internet Verdict after the Initial Check

| | | | |
|---|---|---|---|
| **Owner** — Ashish Raj (PM) | **Reviewer** — Akash (engineering) · Dhruv (product) | **Status** — Draft | **Sign-off** — — |
| **Version** — P2 of 4 · 20 Oct 2026 · split from v1.2-short (from v1.2, signed off 15 Oct 2026) | **Consulted — Solution design** — Ashish Agrawal | **Consulted — Customer OS** — Shail Patel, Manas | |
| **Terminology source** — Wiom Operational Terminology (v0.7) · 15 Oct 2026 · Wiom System Terminology (v0.7) · 15 Oct 2026 | | | |
| **Phase** — P2 of 4 · needs P1 · all 4 PRDs are tested together before release (P1 §2) | | | |

**Designs (Figma)** — screens, flows and copy for this spec:

| App | Design file | Prototype |
|---|---|---|
| CSP app | [CSP app design](https://www.figma.com/design/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%253E-January-2026-Onwards?node-id=18054-2602) | [CSP app prototype](https://www.figma.com/proto/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%3E-January-2026-Onwards?node-id=18054-2603&starting-point-node-id=18054%3A2603&show-proto-sidebar=1&page-id=18054%3A1217) |
| Technician App | [Technician App design](https://www.figma.com/design/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%253E-January-2026-Onwards?node-id=18054-4702) | [Technician App prototype](https://www.figma.com/proto/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%3E-January-2026-Onwards?node-id=18054-4703&starting-point-node-id=18054%3A4703&show-proto-sidebar=1&page-id=18054%3A1217) |
| Customer app — chat and push notification | [Customer chat design](https://www.figma.com/design/8OMg9BTNhVDJxQ5ii10OWj/CA-Final-Dev--%3E-Jan-2026-Onwards-Re.wa.Gh.ka.Net?node-id=17557-48543) | [Customer chat prototype](https://www.figma.com/proto/8OMg9BTNhVDJxQ5ii10OWj/CA-Final-Dev--%3E-Jan-2026-Onwards-Re.wa.Gh.ka.Net?node-id=17557-48543&scaling=contain&content-scaling=fixed) |

Where a design and this spec differ, this spec decides. The designs were reviewed against this spec on 15 Oct 2026, and every difference was decided in v0.31.

---

## 1. Terminology Used

The terms of the P1 PRD apply here. This phase adds none.

---

## 2. Objective & Guardrails

**Objective.** Wiom keeps asking for the Internet Verdict every hour after the Initial Check, so a router that comes back late is seen: the service task then closes at C-04 as "Internet working, no response" instead of at C-05 as "Not verified". The customer gets no new message.

**Only CSPs on the rollout list.** Wiom asks for the Internet Verdict after the Initial Check only for service tasks of CSPs on the rollout list (R12). For a CSP off the list, Wiom asks exactly as today (G8; AC-R12-7).

### Phase and testing

P2 of 4; needs P1 (`STV_P1_Core_Loop_PRD.md`), whose terms, guardrails, lifecycle and rollout list (R12) apply here unchanged. It is built and tested behind the rollout list while it is empty. **Phase gate:** its ACs pass and every P1 AC passes again on the same build. **Full feature test:** before release, the ACs of all 4 PRDs are run together on one build (P1 §2). AC ids are kept from v1.2, so a Given may name an AC in the P1 PRD.


This phase must keep guardrails G3 and G6 (P1 §2).

---

## 3. Stories, Rules & Acceptance Criteria

### R2 — The Internet Verdict *(the parts of R2 in this phase; letters kept from the full rule)*

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R2 | As a customer, I want Wiom to check whether my internet is actually working, so that what I am told matches what is true. | **(c)** On T13, when a positive Internet Verdict is received after the Initial Check and before C-04 runs out from the mark, move to Positive verdict. **(d)** After the Initial Check, ask Connection Validation Service for the Internet Verdict every C-14 until the customer answers, the service task closes, or C-04 runs out. | Count a positive Internet Verdict received after C-04 has run out. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R2-4 | **Given** a service task Resolved at 13:47 on 14 Oct, in No positive verdict since 14:17, **When** a positive Internet Verdict is received at 19:52 and the customer never answers, **Then** it is in Positive verdict at 19:52 and is Closed at 13:47 on 16 Oct, C-04 (48 h) after the mark, with closure reason “Internet working, no response”. | R2c · T13 · R4b · T7 | Settled |
| AC-R2-7 | **Given** a service task in Positive verdict since 14:09, **When** the Internet Verdict at 15:30 is not positive, **Then** it stays in Positive verdict and still auto closes C-04 after the mark. | R2 MUST NOT | Settled |
| AC-R2-8 | **Given** a service task Resolved at 13:47 on 14 Oct that gets no positive Internet Verdict and no answer from the customer, **When** C-04 runs out at 13:47 on 16 Oct, **Then** Wiom asked Connection Validation Service for the Internet Verdict at 13:47, 13:52, 13:57, 14:02, 14:07, 14:12 and 14:17 on 14 Oct, then every hour from 15:17 on 14 Oct to 13:17 on 16 Oct (47 asks), and not after. | R2d · T1 · C-13 · C-14 | Settled |

### R3 — The first message *(the parts of R3 in this phase; letters kept from the full rule)*

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R3 | As a customer, I want one clear question after the CSP's fix, so that I can say whether my internet works. | **(e)** On T13, send the customer nothing new: they already have the first message. | Send more than one first message in a round. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R3-6 | **Given** the service task from AC-R2-4, whose customer got the first message at 14:17, **When** a positive Internet Verdict is received at 19:52, **Then** the customer receives no new message. | R3e · T13 · R3 MUST NOT | Settled |

### R4 — Closure *(the parts of R4 in this phase; letters kept from the full rule)*

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R4 | As a customer, I want my service task to close only when the fix is confirmed or I have had time to say otherwise, so that a closed service task means something. | **(d)** When C-04 runs out with no positive Internet Verdict, stop asking for it (R2d) and keep the service task on C-05; between C-04 and C-05 only the customer's answer or a Wiom CX user's action can change it. | — (R4's MUST NOT is in the P1 PRD.) |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R4-8 | **Given** a service task Resolved at 13:47:00 on 14 Oct with no positive Internet Verdict, and no answer, **When** C-04 (48 h) after the mark passes at 13:47:00 on 16 Oct, **Then** it is still in Pending Verification, not Closed, and waits for C-05 (AC-R4-3); Wiom has stopped asking for the Internet Verdict (AC-R2-8). | R4d · R4 MUST NOT | Settled |

---

## 4. Cross-cutting Acceptance Criteria

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R12-7 | **Given** CSP Om Sai Cable is not on the rollout list, **When** it marks an ISD service task Resolved at 13:47, **Then** Wiom asks Connection Validation Service for the Internet Verdict only as it does today, at the mark, and never again every C-13 or every C-14. | R12b · R12 MUST NOT · G8 · C-14 | Settled |
| AC-BV-2 | **Given** a service task Resolved at 13:47:00, **When** its only positive Internet Verdict comes at 14:17:01, **Then** the customer got the first message at 14:17:00, it moves to Positive verdict at 14:17:01, and no second message is sent. | C-01 · R2b · R2c · R3e | Settled |
| AC-CFG-6 | **Given** C-13 is 5 min and C-14 is 1 h, **When** a Wiom Product user tries to set C-13 to 6 min or C-14 to 5 min, **Then** each change is refused and the values stay 5 min and 1 h. | C-13 range · C-14 range | Settled |
| AC-BV-12 | **Given** a service task Resolved at 13:47:00 on 14 Oct with no positive Internet Verdict since, **When** its only positive Internet Verdict comes at 13:46:59 on 16 Oct, 1 s before C-04 runs out, **Then** it auto closes at 13:47:00 on 16 Oct with “Internet working, no response”. | C-04 · R4b · R2c | Settled |

---

## 5. System Behaviour

Lifecycle: P1 §5. This phase builds T13: No positive verdict → Positive verdict on a positive Internet Verdict after the Initial Check and before C-04 runs out, with no new message (R2c, R3e). When C-04 runs out with no positive Internet Verdict, Wiom stops asking and the service task waits for C-05 (R4d).

**Not possible from here:** No positive verdict × a positive Internet Verdict after C-04 has run out — Wiom has stopped asking, so none can arrive (R2d, R4d).

---

## 6. Configurability

| ID | Parameter | Value / Default | Kind | Range · Owner · Why configurable |
|---|---|---|---|---|
| C-14 | How often Wiom asks Connection Validation Service for the Internet Verdict after the Initial Check, until the customer answers, the service task closes, or C-04 runs out | 1 hour | Configurable | 10 min–1 h · Wiom Product team · PM: every hour after the Initial Check; range set 15 Oct 2026 |
