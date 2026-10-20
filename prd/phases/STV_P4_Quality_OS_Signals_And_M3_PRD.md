# Service Task Verification — P4: Quality OS: signals at Closure and M3

| | | | |
|---|---|---|---|
| **Owner** — Ashish Raj (PM) | **Reviewer** — Akash (engineering) · Dhruv (product) | **Status** — Draft | **Sign-off** — — |
| **Version** — P4 of 5 · 20 Oct 2026 · split from v1.2-short (from v1.2, signed off 15 Oct 2026) | **Consulted — Solution design** — Ashish Agrawal | **Consulted — Customer OS** — Shail Patel, Manas | |
| **Terminology source** — Wiom Operational Terminology (v0.7) · 15 Oct 2026 · Wiom System Terminology (v0.7) · 15 Oct 2026 | | | |
| **Phase** — P4 of 5 · needs P1 · all five PRDs are tested together before release (P1 §2) | | | |

**Designs (Figma)** — screens, flows and copy for this spec:

| App | Design file | Prototype |
|---|---|---|
| CSP app | [CSP app design](https://www.figma.com/design/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%253E-January-2026-Onwards?node-id=18054-2602) | [CSP app prototype](https://www.figma.com/proto/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%3E-January-2026-Onwards?node-id=18054-2603&starting-point-node-id=18054%3A2603&show-proto-sidebar=1&page-id=18054%3A1217) |
| Technician App | [Technician App design](https://www.figma.com/design/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%253E-January-2026-Onwards?node-id=18054-4702) | [Technician App prototype](https://www.figma.com/proto/W2Z3B5xfFO3UibJSzkyHn2/PA---Dev--%3E-January-2026-Onwards?node-id=18054-4703&starting-point-node-id=18054%3A4703&show-proto-sidebar=1&page-id=18054%3A1217) |
| Customer app — chat and push notification | [Customer chat design](https://www.figma.com/design/8OMg9BTNhVDJxQ5ii10OWj/CA-Final-Dev--%3E-Jan-2026-Onwards-Re.wa.Gh.ka.Net?node-id=17557-48543) | [Customer chat prototype](https://www.figma.com/proto/8OMg9BTNhVDJxQ5ii10OWj/CA-Final-Dev--%3E-Jan-2026-Onwards-Re.wa.Gh.ka.Net?node-id=17557-48543&scaling=contain&content-scaling=fixed) |

Where a design and this spec differ, this spec decides. The designs were reviewed against this spec on 15 Oct 2026, and every difference was decided in v0.31.

---

## 1. Terminology Used

The terms of the P1 PRD apply here. This phase adds:

| Term | Meaning | Owner (domain) |
|---|---|---|
| Within TAT | Whether a service task was fixed in time, judged when it is Closed: its CSP TAT — the time the CSP spent on it, In Progress rounds only, inside CSP working hours (11:00–21:00 IST, C-06), with every hour in Pending Verification left out — compared with the TAT for its subtype (C-15). Within TAT when the CSP TAT is no longer than C-15; otherwise breached. It is the same time the SLA timer in the CSP app counts, and it is judged the same way however the service task closes — the customer's yes, a Wiom CX user's close, or an auto closure at C-04 or C-05. Sent to Quality OS in the resolution signal. | Support Resolution OS |
| M3 timer | The clock M3 runs on each service task. It starts when the service task is raised, counts every hour of the day, pauses while the service task is in Pending Verification, and resumes where it stopped on a Reopen. | Quality OS |
| M3 cycle | The fixed half-month period M3 counts in (C-09). Each service task counts in at most one. | Quality OS |

---

## 2. Objective & Guardrails

**Objective.** A CSP's quality score rests on what happened to the customer: Quality OS hears about a service task only at Closure, with its closure reason and a within-TAT judgement on the CSP's own time, and M3 never counts time spent in Pending Verification.

### Phase and testing

P4 of 5; needs P1 (`STV_P1_Core_Loop_PRD.md`), whose terms, guardrails, lifecycle and rollout list (R12) apply here unchanged. It is built and tested behind the rollout list while it is empty. **Phase gate:** its ACs pass and every P1 AC passes again on the same build. **Full feature test:** before release, the ACs of all five PRDs are run together on one build (P1 §2). AC ids are kept from v1.2, so a Given may name an AC in the P1 PRD.

### Guardrails this phase builds

| ID | Guardrail | One line | Anchors |
|---|---|---|---|
| G4 | **Signals only at Closed** | Quality OS hears about a service task's resolution only when it is Closed, never at the Resolved mark. | R6 · AC-R6-1 · AC-DUP-1 |
| G5 | **Verification never counts against the CSP** | Time in Pending Verification never counts in the CSP TAT, in the within-TAT judgement sent to Quality OS, or on the M3 timer. | R1 · R6 · R7 · R10 · AC-R1-2 · AC-R6-4 · AC-R6-5 · AC-R7-1 · AC-R10-2 |

---

## 3. Stories, Rules & Acceptance Criteria

### R6 — Signals at Closure

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R6 | As a CSP, I want my quality score to rest on what happened to the customer, so that a confirmed fix counts and a claimed one does not. | **(a)** At every Closure, send Quality OS the resolution signal with the closure reason. **(b)** For “Not verified”, never signal the service task as fixed. **(c)** For “Closed by the Wiom CX team”, signal the service task as fixed. **(d)** On T15, send whatever signal is sent today; this spec adds none. **(e)** At every Closure — the customer's yes, a Wiom CX user's close, or an auto closure at C-04 or C-05 — include in the resolution signal whether the service task was within TAT, judged the same way each time: its CSP TAT at Closure — In Progress rounds only, inside CSP working hours (C-06), summed over every round, with all time in Pending Verification left out — compared with the TAT for its subtype (C-15). | Send any resolution signal at the Resolved mark or during Pending Verification. Count any time in Pending Verification, or outside CSP working hours, toward the within-TAT judgement. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R6-1 | **Given** the service task from AC-R1-1, Resolved at 13:47 on 14 Oct, **When** it is Closed at 18:32 with closure reason “Customer confirmed”, **Then** Quality OS receives one resolution signal at 18:32 carrying “Customer confirmed”, and received none between 13:47 and 18:32. | R6a · T5 · G4 | Settled |
| AC-R6-3 | **Given** the service task from AC-R4-11, **When** a Wiom CX user closes it in Kapture at 17:20 on 14 Oct, **Then** Quality OS receives one resolution signal at 17:20, carrying “Closed by the Wiom CX team” and marking it as fixed. | R6a · R6c · T14 | Settled |
| AC-R6-4 | **Given** an ISD service task that reaches CSP Shiv Net at 15:00 on 14 Oct, **When** the CSP marks it Resolved at 16:00, it is Reopened at 17:00, the CSP marks it Resolved again at 19:00, and the customer says yes at 20:00, **Then** it is Closed at 20:00 and Quality OS receives the resolution signal at 20:00 marked within TAT: CSP TAT 3 h (15:00–16:00 and 17:00–19:00), under C-15 (4 working hours); the 16:00–17:00 and 19:00–20:00 hours in Pending Verification are not counted. | R6e · R7 · G5 · T5 | Settled |
| AC-R6-5 | **Given** an ISD service task that reaches CSP Shiv Net at 19:30 on 14 Oct, **When** the CSP marks it Resolved at 20:30, the customer says No at 22:00 (outside working hours), the CSP marks it Resolved again at 13:30 on 15 Oct, and the customer says yes at 15:00, **Then** Quality OS receives the resolution signal at 15:00 on 15 Oct marked within TAT: CSP TAT 3 h 30 min (19:30–20:30 on 14 Oct, then 11:00–13:30 on 15 Oct, because the TAT waits for working hours after the 22:00 Reopen), under C-15; neither Pending Verification (20:30–22:00, 13:30–15:00) nor the night counts, although 19 h 30 min passed on the clock. | R6e · R5d · R7 · C-06 · G5 · T6 | Settled |
| AC-R6-6 | **Given** the service task from AC-R6-5, **When** the CSP marks it Resolved again at 14:30 on 15 Oct instead of 13:30, and the customer says yes at 15:00, **Then** the signal is marked breached: CSP TAT 4 h 30 min (1 h plus 3 h 30 min), over C-15. | R6e · C-15 | Settled |
| AC-R6-7 | **Given** C-15 is 4 working hours, **When** one service task is Closed with a CSP TAT of exactly 4 h 00 min and another with 4 h 01 min, **Then** the first is marked within TAT and the second breached. | R6e · C-15 | Settled |
| AC-R6-8 | **Given** an ISD service task that reaches CSP Shiv Net at 11:20 on 14 Oct and is marked Resolved at 13:47, with a positive Internet Verdict at 14:09 and no answer from the customer, **When** it auto closes at 13:47 on 16 Oct, **Then** Quality OS receives the resolution signal at 13:47 on 16 Oct marked within TAT: CSP TAT 2 h 27 min, the same as the CSP app SLA timer showed at the mark; the 48 h in Pending Verification are not counted. | R6e · R7 · G5 · T7 | Settled |
| AC-R6-9 | **Given** an ISD service task that reaches CSP Shiv Net at 11:20 on 14 Oct and is marked Resolved at 13:47, with no positive Internet Verdict and no answer, **When** it auto closes at 13:47 on 17 Oct with closure reason “Not verified”, **Then** the resolution signal at 13:47 on 17 Oct is marked within TAT (CSP TAT 2 h 27 min), and still does not mark the service task as fixed (R6b). | R6e · R6b · G5 · T8 | Settled |

### R10 — M3 counts only time waiting on the CSP

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R10 | As a CSP, I want M3 to count only the time a service task waits on me, so that Wiom checking a fix never makes a service task look long-open. | **(a)** Start the M3 timer when the service task is raised, and run it on the wall clock, every hour of the day. **(b)** On T1, pause the M3 timer at the Resolved mark. **(c)** On T6, resume the M3 timer from where it stopped. **(d)** On T12, when the M3 timer reaches C-08, count the service task once toward M3, in the M3 cycle (C-09) in which that happens. **(e)** On T5, T7, T8, T14 and T15, stop the M3 timer; a service task whose timer never reached C-08 is never counted. **(f)** Give a CSP a PASS on M3 for a cycle when C-10 or fewer of its service tasks were counted in it, and a FAIL otherwise. | Count time in Pending Verification on the M3 timer. Count the same service task in more than one M3 cycle, even if it stays open or is Reopened. Pause the M3 timer for any reason other than Pending Verification. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R10-1 | **Given** an ISD service task raised at 20:10 on 9 Oct and never marked Resolved, **When** its M3 timer reaches C-08 (48 h) at 20:10 on 11 Oct, **Then** it is counted once toward M3 in the 1–15 Oct cycle, although its CSP TAT, counted in working hours only, is 20 h. | R10a · R10d · T12 | Settled |
| AC-R10-2 | **Given** a service task raised at 10:20 on 14 Oct and marked Resolved at 06:20 on 15 Oct (M3 timer 20 h), with no positive Internet Verdict since the mark, **When** the customer replies yes at 08:05 on 17 Oct and it is Closed, **Then** its M3 timer reads 20 h and it is never counted toward M3. | R10b · R10e · T1 · T5 · G5 | Settled |
| AC-R10-3 | **Given** a service task raised at 09:20 on 14 Oct, marked Resolved at 15:20 on 15 Oct (M3 timer 30 h) and Reopened at 09:05 on 16 Oct, **When** it is still not marked Resolved again at 03:05 on 17 Oct, **Then** its M3 timer reaches 48 h at 03:05 on 17 Oct and it is counted once in the 16–31 Oct cycle. | R10c · R10d · T6 · T12 | Settled |
| AC-R10-4 | **Given** a service task counted toward M3 at 22:40 on 14 Oct in the 1–15 Oct cycle, **When** it is still open on 16 Oct, is marked Resolved at 09:00 on 17 Oct and is Reopened at 11:30 on 18 Oct, **Then** it is not counted in the 16–31 Oct cycle. | R10 MUST NOT | Settled |
| AC-R10-5 | **Given** a CSP with 2 service tasks counted toward M3 in the 1–15 Oct cycle, **When** Quality OS evaluates that cycle, **Then** the CSP's M3 result is PASS. | R10f | Settled |
| AC-R10-6 | **Given** a CSP with 3 service tasks counted toward M3 in the 1–15 Oct cycle, **When** Quality OS evaluates that cycle, **Then** the CSP's M3 result is FAIL. | R10f | Settled |
| AC-R10-7 | **Given** a service task raised at 08:00 on 14 Oct and In Progress, **When** 21:00 on 14 Oct passes and the CSP's working hours end, **Then** its M3 timer keeps running through the night and reads 25 h at 09:00 on 15 Oct. | R10a · R10 MUST NOT | Settled |

---

## 4. Cross-cutting Acceptance Criteria

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R8-8 | **Given** an ISD service task of CSP Shiv Net, on the rollout list, In Progress since 11:20 on 14 Oct and never marked Resolved, M3 timer at 3 h 10 min, **When** a Wiom CX user closes it in Kapture at 14:30, **Then** it is Closed at 14:30 without Pending Verification, with the same closure reason, signals, customer message and CSP notifications a Kapture close produces today; it does not get “Closed by the Wiom CX team”; and its M3 timer stops at 3 h 10 min. | R8e · R4h · R6d · R10e · R12a · T15 · G2 | Settled |
| AC-R12-5 | **Given** CSP Om Sai Cable is not on the rollout list, **When** Quality OS counts M3 for it, **Then** it uses today's rolling window and today's clock, with no pause and no fixed M3 cycles. | R12b · R10 · G8 | Settled |
| AC-BV-7 | **Given** a service task whose M3 timer reads 47 h 59 min when the CSP marks it Resolved, **When** it is later Closed with “Customer confirmed”, **Then** it is not counted toward M3. | C-08 · R10e | Settled |
| AC-BV-8 | **Given** a service task In Progress, **When** its M3 timer reaches 48 h 00 min at 23:59 on 15 Oct, **Then** it is counted in the 1–15 Oct cycle, not the 16–31 Oct cycle. | C-09 · R10d | Settled |
| AC-BV-9 | **Given** a service task In Progress, **When** its M3 timer reaches 48 h 00 min at 00:00 on 16 Oct, **Then** it is counted in the 16–31 Oct cycle. | C-09 · R10d | Settled |
| AC-RACE-3 | **Given** a service task In Progress whose M3 timer reaches 48 h at 13:47:00, **When** the CSP marks it Resolved at 13:47:00, **Then** it is counted toward M3. | Precedence · T1 · T12 | Settled |
| AC-DUP-1 | **Given** a service task in Pending Verification, **When** the customer replies yes twice, at 15:02 and 15:03, **Then** it is Closed once at 15:02, and Quality OS receives one signal. | T5 · R6a | Settled |

---

## 5. System Behaviour

Lifecycle: P1 §5. This phase builds T12 (In Progress → In Progress: the M3 timer reaches C-08, counted once) and adds to T1, T5, T6, T7, T8, T14 and T15 what Quality OS hears and what the M3 timer does (R6, R10).

**Precedence:** the M3 timer reaching C-08 at the same instant as a Resolved mark is applied before the mark (AC-RACE-3).

---

## 6. Configurability

| ID | Parameter | Value / Default | Kind | Range · Owner · Why configurable |
|---|---|---|---|---|
| C-08 | M3 long-open limit: M3 timer time, wall clock, after which a service task counts toward M3 | 48 hours | Fixed | — |
| C-09 | M3 cycles | 1st–15th, and 16th–last day of each month (IST) | Fixed | — |
| C-10 | M3 pass mark: most service tasks counted in one cycle for a PASS | 2 | Fixed | — |
| C-15 | TAT for an ISD or RDNI service task: the most CSP TAT that still counts as within TAT | 4 working hours | Fixed — unchanged by this spec | — · today's per-subtype value in Support Resolution OS (`SR_SLA_NO_INTERNET`, `SR_SLA_RECHARGE_DONE_NO_INTERNET`: 4 h, working-hours basis) |

**Interaction note (C-08 × C-04 / C-05):** the M3 timer runs only outside Pending Verification and the verification windows run only inside it, so the two never overlap. A service task can wait the full 72 hours of C-05 without moving closer to C-08.
