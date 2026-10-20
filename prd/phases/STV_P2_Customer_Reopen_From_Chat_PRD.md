# Service Task Verification — P2: Customer reopens from the chat

| | | | |
|---|---|---|---|
| **Owner** — Ashish Raj (PM) | **Reviewer** — Akash (engineering) · Dhruv (product) | **Status** — Draft | **Sign-off** — — |
| **Version** — P2 of 5 · 20 Oct 2026 · split from v1.2-short (from v1.2, signed off 15 Oct 2026) | **Consulted — Solution design** — Ashish Agrawal | **Consulted — Customer OS** — Shail Patel, Manas | |
| **Terminology source** — Wiom Operational Terminology (v0.7) · 15 Oct 2026 · Wiom System Terminology (v0.7) · 15 Oct 2026 | | | |
| **Phase** — P2 of 5 · needs P1 · all five PRDs are tested together before release (P1 §2) | | | |

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

**Objective.** A customer whose internet still does not work can send the same service task back to the same CSP from the chat, by tapping नहीं and choosing to reopen the previous service task; any other choice changes nothing, and after Closure the chat offers a new service task instead.

**Only CSPs on the rollout list.** The chat choices in this phase appear only for service tasks of CSPs on the rollout list (R12). For a CSP off the list the chat works exactly as today (G8; AC-R12-6).

### Phase and testing

P2 of 5; needs P1 (`STV_P1_Core_Loop_PRD.md`), whose terms, guardrails, lifecycle and rollout list (R12) apply here unchanged. It is built and tested behind the rollout list while it is empty. **Phase gate:** its ACs pass and every P1 AC passes again on the same build. **Full feature test:** before release, the ACs of all five PRDs are run together on one build (P1 §2). AC ids are kept from v1.2, so a Given may name an AC in the P1 PRD.


This phase must keep guardrails G1, G6 and G7 (P1 §2).

---

## 3. Stories, Rules & Acceptance Criteria

### R5 — Reopen on the customer's no *(the parts of R5 in this phase; letters kept from the full rule)*

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R5 | As a customer whose internet still does not work, I want my previous service task reopened with the same CSP when I ask for it, or a way to raise a new one, so that I choose how to go on. | On T6: **(a)** move the service task back to In Progress at once, whatever the Internet Verdict, when the customer answers "not working" and chooses "पिछली शिकायत फिर से दर्ज करें"; **(g)** When the customer taps नहीं and chooses "नई शिकायत दर्ज करें", direct the customer to call the Wiom CX team; this is not a No, and the service task carries on as if the customer had not replied. **(h)** When the customer taps नहीं and chooses neither, it counts for nothing: the service task carries on exactly as if the customer had not replied. | Reopen on a नहीं alone, before the customer chooses to reopen the previous service task. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R5-1 | **Given** the service task from AC-R2-1, positive Internet Verdict at 14:09, **When** Rekha Verma answers "नहीं, बंद है" at 16:04 and chooses "पिछली शिकायत फिर से दर्ज करें" at 16:05 on 14 Oct, **Then** the service task is In Progress at 16:05, with CSP Shiv Net, under the same service task. | R5a · T6 · G7 | Settled |
| AC-R5-2 | **Given** the Reopen in AC-R5-1, **When** the CSP opens the service task at 16:10, **Then** the CSP app shows it as the same issue, Reopened, not as a new service task. | R5b · T6 | Settled |
| AC-R5-13 | **Given** the service task from AC-R5-1, worked by Technician Suresh in an earlier round and last assigned to Technician Ravi, **When** it is Reopened at 16:05, **Then** it is back in Ravi's Technician App list and in Shiv Net's CSP app at 16:05; Shiv Net's Owner, Manager and Manager+ receive a CSP app notification, and Ravi the same notification in the Technician App, at 16:05 that the service task is back to be fixed; and no notification or screen says that the customer reopened it; Suresh gets nothing. Had it been last assigned to the CSP's owner, it would be back in the CSP app only. | R5b · R5f · T6 · R5 MUST NOT | Settled |
| AC-R5-5 | **Given** a service task of CSP Shiv Net in Pending Verification with CSP TAT 2 h 27 min, **When** the customer says No at 22:30 on 14 Oct, **Then** it is In Progress at 22:30, Shiv Net and the assigned Technician receive their Reopen notifications at 22:30, and its CSP TAT reads 2 h 27 min until 11:00 on 15 Oct, then grows. (Its M3 timer resumes at once, on the wall clock: AC-R10-3, AC-R10-7.) | R5d · R5f · T6 | Settled |
| AC-R5-6 | **Given** the Reopened service task from AC-R5-1, **When** the CSP marks it Resolved again at 19:12, **Then** it is in Pending Verification with a new Initial Check ending at 19:42 and a new Internet Verdict. | R5e · T1 | Settled |
| AC-R5-10 | **Given** a service task in Pending Verification, **When** Rekha Verma answers "नहीं, बंद है" at 16:04 and chooses "नई शिकायत दर्ज करें" at 16:05, **Then** the chat directs her to call the Wiom CX team, and the service task is still in Pending Verification at 16:05, unchanged. | R5g · R5 MUST NOT | Settled |
| AC-R5-11 | **Given** a service task Resolved at 13:47 on 14 Oct, with a positive Internet Verdict at 14:09, **When** Rekha Verma taps नहीं at 16:04 and chooses neither option, **Then** it stays in Pending Verification and, with nothing else, auto closes at 13:47 on 16 Oct with “Internet working, no response”, as if she had not replied. | R5h · R4b · T7 | Settled |

### R9 — After Closure *(the parts of R9 in this phase; letters kept from the full rule)*

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R9 | As a customer whose internet fails again after my service task closed, I want a new service task raised for me, so that the fault is fixed again. | **(c)** When the customer tries to reopen a Closed service task — for example by choosing "पिछली शिकायत फिर से दर्ज करें" in the chat after it closed — do not reopen it; tell the customer it is closed and offer a new service task instead. | Reopen or change a Closed service task. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R9-4 | **Given** the service task from AC-R4-2, Closed at 13:47 on 16 Oct, **When** Rekha Verma answers "नहीं, बंद है" in the chat at 18:00 on 16 Oct and chooses "पिछली शिकायत फिर से दर्ज करें", **Then** it stays Closed, and the chat tells her it is closed and offers to raise a new service task. | R9c · R9 MUST NOT · G1 | Settled |

---

## 4. Cross-cutting Acceptance Criteria

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R8-3 | **Given** the service task from AC-R5-1, Reopened at 16:05 on 14 Oct, **When** a Wiom CX user opens it in Kapture at 16:30, **Then** it shows Status Pending and Sub Status Open, not Sub Status Resolved. | R8c · T6 | Settled |
| AC-R8-10 | **Given** the service task from AC-R5-1, Reopened at 16:05 on 14 Oct and shown Open in Kapture, **When** no one acts on it until 20:05 on 14 Oct, **Then** it is still In Progress with CSP Shiv Net, not Completed. | R8 MUST NOT · G6 | Settled |
| AC-R12-6 | **Given** CSP Om Sai Cable is not on the rollout list and its ISD service task closed at its Resolved mark at 13:47 as today, **When** the customer opens the chat at 16:00, **Then** the chat offers only what it offers today, with no नहीं or "पिछली शिकायत फिर से दर्ज करें" choice from this spec, and any reopen follows today's handling. | R12b · R12 MUST NOT · G8 · R5a | Settled |
| AC-WF-2 | **Given** an ISD service task In Progress from 11:20 on 14 Oct, marked Resolved by the Technician at 13:47, **When** no positive Internet Verdict comes by 14:17, the customer says No at 16:05, the CSP marks it Resolved again at 19:12, a positive Internet Verdict comes at 19:20, and no answer comes, **Then** it is Closed at 19:12 on 16 Oct (C-04 after the second Resolved mark) with closure reason “Internet working, no response” and CSP TAT 5 h 34 min. | T1 · T3 · T6 · T2 · T7 · G5 | Settled |
| AC-RACE-1 | **Given** a positive Internet Verdict and no answer, **When** the customer says No at the same instant C-04 runs out, **Then** the service task is Reopened, not Closed. | Precedence · T6 · T7 | Settled |
| AC-RACE-5 | **Given** a service task in Pending Verification, **When** the customer says No at the same instant a Wiom CX user closes it in Kapture, **Then** it is Reopened, not Closed. | Precedence · T6 · T14 | Settled |
| AC-DUP-2 | **Given** a service task in Pending Verification, **When** the customer says No twice, at 16:05 and 16:06, **Then** it is Reopened once, at 16:05. | T6 · R5a | Settled |

---

## 5. System Behaviour

Some ACs here also check P1 behaviour: the same issue shown to the CSP (R5b), the new Pending Verification (R5e) and the Reopen notifications (R5f) are built in P1 for a Wiom CX user's Reopen, and first run at this gate because their ACs start from the customer's own No in the chat.

Lifecycle: P1 §5. This phase builds T6 by the customer's own choice in the chat: नहीं, then "पिछली शिकायत फिर से दर्ज करें", from any stage of Pending Verification. A नहीं with the other choice, or with none, changes nothing (R5g, R5h). After Closure, the reopen choice changes nothing; the chat offers a new service task (R9c).

**Precedence:** a customer answer at the same instant as C-04 or C-05 running out, or as a Wiom CX user's Kapture close, is applied first (AC-RACE-1, AC-RACE-5).

---

## 6. Configurability

No parameters of its own; P1's apply.
