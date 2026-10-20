# Service Task Verification — P5: IVR PIN until Closure

| | | | |
|---|---|---|---|
| **Owner** — Ashish Raj (PM) | **Reviewer** — Akash (engineering) · Dhruv (product) | **Status** — Draft | **Sign-off** — — |
| **Version** — P5 of 5 · 20 Oct 2026 · split from v1.2-short (from v1.2, signed off 15 Oct 2026) | **Consulted — Solution design** — Ashish Agrawal | **Consulted — Customer OS** — Shail Patel, Manas | |
| **Terminology source** — Wiom Operational Terminology (v0.7) · 15 Oct 2026 · Wiom System Terminology (v0.7) · 15 Oct 2026 | | | |
| **Phase** — P5 of 5 · needs P1 · all 5 PRDs are tested together before release (P1 §2) | | | |

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

**Objective.** The customer can keep calling their CSP through the IVR until the service task is Closed: the IVR PIN stays allocated through Pending Verification and every Reopen, and is released at Closure, not at the Resolved mark.

**Only CSPs on the rollout list.** The IVR PIN is kept until Closure only for service tasks of CSPs on the rollout list (R12). For a CSP off the list, it is released exactly as today (G8; AC-R13-5).

### Phase and testing

P5 of 5; needs P1 (P1: `STV_P1_Core_Loop_PRD.md`), whose terms, guardrails, lifecycle and rollout list (R12) apply here unchanged. It is built and tested behind the rollout list while it is empty. **Phase gate:** its ACs pass and every P1 AC passes again on the same build. **Full feature test:** before release, the ACs of all 5 PRDs are run together on one build (P1 §2). AC ids are kept from v1.2, so a Given may name an AC in the P1 PRD.


This phase must keep guardrails G1 and G8 (P1 §2).

---

## 3. Stories, Rules & Acceptance Criteria

### R13 — The IVR PIN stays until Closure

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R13 | As a customer whose fix is being verified, I want to keep reaching my CSP through the IVR until my service task is Closed, so that I can call while Wiom is still checking or after a Reopen. | **(a)** Keep the service task's IVR PIN allocated through Pending Verification and every Reopen, and release it at Closure (T5, T7, T8, T14), as a Closure releases it today. **(b)** For a Kapture close before any Resolved mark (T15), and for a CSP not on the rollout list, release it as today. | Release the IVR PIN at the Resolved mark or when a Netbox swap commits. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R13-1 | **Given** the service task from AC-R1-1, Resolved at 13:47 on 14 Oct, **When** Rekha Verma calls the IVR with its PIN at 15:00, **Then** the PIN is still allocated and the call goes through as before the mark. | R13a · R13 MUST NOT · T1 | Settled |
| AC-R13-2 | **Given** the service task from AC-R5-1, Reopened at 16:05, **When** Rekha Verma calls the IVR with its PIN at 16:30, **Then** the PIN is still allocated. | R13a · T6 | Settled |
| AC-R13-3 | **Given** the service task from AC-R4-1, **When** it is Closed with “Customer confirmed” at 18:32, **Then** its IVR PIN is released at 18:32, and not before. | R13a · T5 | Settled |
| AC-R13-4 | **Given** the service task from AC-R1-9, **When** the Netbox swap commits at 13:47, **Then** its IVR PIN is still allocated after 13:47. | R13 MUST NOT · R1e | Settled |
| AC-R13-5 | **Given** CSP Om Sai Cable is not on the rollout list, **When** one of its service tasks closes at its Resolved mark at 13:47, **Then** its IVR PIN is released as today. | R13b · R12b · G8 | Settled |

---

## 4. Cross-cutting Acceptance Criteria

None in this phase.

---

## 5. System Behaviour

Lifecycle: P1 §5, unchanged. This phase only decides when the IVR PIN is released: at Closure (T5, T7, T8, T14), not at T1; through T6 it stays allocated.

---

## 6. Configurability

No parameters of its own; P1's apply.
