# Service Task Verification — P4: Records: Kapture comments and notification tracking

| | | | |
|---|---|---|---|
| **Owner** — Ashish Raj (PM) | **Reviewer** — Akash (engineering) · Dhruv (product) | **Status** — Draft | **Sign-off** — — |
| **Version** — P4 of 4 · 20 Oct 2026 · split from v1.2-short (from v1.2, signed off 15 Oct 2026) | **Consulted — Solution design** — Ashish Agrawal | **Consulted — Customer OS** — Shail Patel, Manas | |
| **Terminology source** — Wiom Operational Terminology (v0.7) · 15 Oct 2026 · Wiom System Terminology (v0.7) · 15 Oct 2026 | | | |
| **Phase** — P4 of 4 · needs P1; P2 for one comment (K4) · all 4 PRDs are tested together before release (P1 §2) | | | |

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

**Objective.** Every action in Verification leaves a record: a Kapture comment the Wiom CX team can read, and a record of every notification and chat answer in the Customer app and the CSP app. Records never change behaviour.

**Only CSPs on the rollout list.** Kapture comments and the tracking records are made only for service tasks of CSPs on the rollout list (R12). For a CSP off the list, Kapture and the records stay exactly as today (G8; AC-R12-8).

### Phase and testing

P4 of 4; needs P1; P2 for one comment (K4) (P1: `STV_P1_Core_Loop_PRD.md`), whose terms, guardrails, lifecycle and rollout list (R12) apply here unchanged. It is built and tested behind the rollout list while it is empty. **Phase gate:** its ACs pass and every P1 AC passes again on the same build. **Full feature test:** before release, the ACs of all 4 PRDs are run together on one build (P1 §2). AC ids are kept from v1.2, so a Given may name an AC in the P1 PRD.


This phase must keep guardrails G6 (P1 §2).

---

## 3. Stories, Rules & Acceptance Criteria

### R8 — The Wiom CX team in Kapture *(the parts of R8 in this phase; letters kept from the full rule)*

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R8 | As a Wiom CX user, I want Kapture to show live where a service task stands in Verification and every action taken on it, so that I can tell a customer the truth and act on their call. | **(d)** Add a Kapture comment for every action in this spec, at the moment it happens, with exactly the text in the table *Kapture status and comment per action* below (K1–K14), filling each `{…}` with the actual value. | Show the CSP or the Technician what the Kapture comments say about the verdict. |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R8-4 | **Given** the service task from AC-R2-1, marked Resolved by Technician Ravi at 13:47 on 14 Oct, first message sent at 14:09, customer's yes at 18:32, **When** a Wiom CX user opens it in Kapture at 18:40, **Then** it has three comments in order, with the K1 text at 13:47 ("Marked Resolved by Technician Ravi at 13:47. Wiom is now verifying the fix…"), the K2 text at 14:09 and the K5 text at 18:32. | R8d · T1 · T2 · T5 | Settled |
| AC-R8-5 | **Given** the service task from AC-R5-1, **When** Rekha Verma says No at 16:05 on 14 Oct, **Then** its Kapture record has the K6 comment at 16:05: the customer said it is not working and the service task is back with CSP Shiv Net. | R8d · T6 | Settled |
| AC-R8-6 | **Given** the service task from AC-R4-11, **When** a Wiom CX user closes it in Kapture at 17:20 on 14 Oct, **Then** its Kapture record shows Status Complete / Sub Status Completed and the K10 comment at 17:20, with the closure reason. | R8b · R8d · T14 | Settled |
| AC-R8-11 | **Given** a service task Resolved at 13:47 on 14 Oct with no positive Internet Verdict, whose first-message push notification could not be sent at 14:17, and no answer, **When** a Wiom CX user opens it in Kapture at 13:50 on 15 Oct, **Then** it has the K1 comment at 13:47 and the K3 comment at 14:17, and no comment about the push notification that could not be sent. | R8d · T1 · T3 | Settled |
| AC-R8-12 | **Given** the service task from AC-R4-2, auto closed at 13:47 on 16 Oct, **When** a Wiom CX user opens it in Kapture at 14:00, **Then** it has the K8 comment at 13:47: Closed with closure reason “Internet working, no response”, and CSP Shiv Net and the assigned Technician notified. | R8d · T7 | Settled |
| AC-R8-16 | **Given** the service task from AC-R4-3, Resolved at 13:47 on 14 Oct, never a positive Internet Verdict, no answer, **When** it auto closes at 13:47 on 17 Oct, **Then** its Kapture record has a comment at 13:47 with the K9 text, the durations filled as 72 h and 48 h. | R8d · T8 | Settled |
| AC-R8-18 | **Given** the service task from AC-R2-4, **When** a positive Internet Verdict comes at 19:52 on 14 Oct, **Then** its Kapture record has the K4 comment at 19:52. | R8d · T13 | Settled |
| AC-R8-19 | **Given** the service task from AC-R5-3 and another in Pending Verification, **When** a Wiom CX user records the customer asking to reopen the first at 17:40, and sets the second to Open in Kapture at 17:45, **Then** the first has the K7 "recorded … asked to reopen" comment at 17:40, and the second the K7 "set the ticket to Open" comment at 17:45. | R8d · T6 | Settled |
| AC-R8-20 | **Given** the service task from AC-R4-2, Closed at 13:47 on 16 Oct, **When** a Wiom CX user tries to reopen it in Kapture at 09:15 on 2 Nov, **Then** its Kapture record has the K11 comment at 09:15 and it stays Closed. | R8d · R8 MUST NOT | Settled |
| AC-R8-21 | **Given** the service task from AC-R5-10, **When** Rekha Verma chooses "नई शिकायत दर्ज करें" at 16:05, **Then** its Kapture record has the K12 comment at 16:05. | R8d · R5g | Settled |
| AC-R8-22 | **Given** the service task from AC-R5-11, **When** Rekha Verma taps नहीं at 16:04 and chooses neither option, **Then** its Kapture record has the K13 comment at 16:04. | R8d · R5h | Settled |
| AC-R8-23 | **Given** the service task from AC-R9-4, **When** Rekha Verma tries to reopen it in the chat at 18:00 on 16 Oct, **Then** its Kapture record has the K14 comment at 18:00. | R8d · R9c | Settled |
| AC-R8-24 | **Given** a service task with the K1 to K3 comments in Kapture, **When** the CSP and the Technician open it in the CSP app and the Technician App, **Then** neither app shows any Kapture comment or the Internet Verdict. | R8 MUST NOT | Settled |
| AC-R8-25 | **Given** a service task in Pending Verification, **When** the customer replies yes twice, at 15:02 and 15:03, **Then** its Kapture record has one K5 comment, at 15:02. | R8d · T5 | Settled |

### R11 — Notifications are tracked in the Customer app and the CSP app

| ID | Story | MUST | MUST NOT |
|---|---|---|---|
| R11 | As the Wiom Product team, I want every notification in Verification tracked in the Customer app and the CSP app, so that we can see whether customers, CSPs and Technicians get, click and act on them. | **(a)** On T2, T3 and T14, record each push notification sent to the customer — the first message, and the closure message at a Kapture close — with the service task, the message type and the time sent. **(b)** Record when each push notification to the customer is delivered and when it is clicked. **(c)** Record each answer the customer gives in the chat — yes, not working, and the choice to reopen the previous service task or raise a new one — against the same service task. **(d)** Record a push notification that could not be sent. **(e)** In the CSP app and the Technician App, record each Reopen and Closure notification sent to the CSP and the assigned Technician (R5f, R4f), with the service task, the type and the time sent, and when it is delivered and when it is clicked, or that it could not be sent. | Delay or hold back a message while it is being recorded. Let whether a notification was delivered or clicked change when the service task closes; sent still counts as told (R3d). |

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R11-2 | **Given** the service task from AC-R2-1, **When** the first-message push notification goes to Rekha Verma at 14:09, reaches her phone at 14:10 and she clicks it at 14:31, **Then** records exist for that service task with message type "first message", sent at 14:09, delivered at 14:10 and clicked at 14:31. | R11a · R11b · T2 | Settled |
| AC-R11-3 | **Given** a first message sent at 14:17, **When** the customer answers "नहीं, बंद है" in the chat at 14:51 and chooses "पिछली शिकायत फिर से दर्ज करें" at 14:52, **Then** records for that service task show the answer "not working" at 14:51 and the reopen choice at 14:52. | R11c · T6 | Settled |
| AC-R11-4 | **Given** a customer whose phone has no app notifications, **When** the first message is due at 14:17, **Then** a record shows the push notification could not be sent at 14:17. | R11d · R3c | Settled |
| AC-R11-5 | **Given** a first-message push notification sent at 14:09 on 14 Oct that the customer never opens, **When** C-04 (48 h) after the mark runs out at 13:47 on 16 Oct, **Then** the service task auto closes at 13:47 on 16 Oct as usual. | R11 MUST NOT | Settled |
| AC-R11-7 | **Given** the Reopen notification from AC-R5-13, sent at 16:05, **When** it is delivered to the CSP's phone at 16:06 and clicked in the CSP app at 16:20, **Then** records exist for that service task showing a "reopened" notification sent at 16:05, delivered at 16:06 and clicked at 16:20. | R11e · T6 | Settled |
| AC-R11-8 | **Given** the Closure notification from AC-R4-10, sent at 13:47 on 16 Oct and delivered at 13:48, **When** the CSP never clicks it, **Then** a record exists for that service task showing a "closed" notification sent at 13:47, delivered at 13:48 and not clicked. | R11e · T7 | Settled |
| AC-R11-9 | **Given** the Reopen from AC-R5-13, **When** the assigned Technician's phone receives the Technician App notification at 16:06 and the Technician clicks it at 16:12, **Then** records exist for that service task showing a "reopened" Technician App notification sent at 16:05, delivered at 16:06 and clicked at 16:12. | R11e · T6 | Settled |
| AC-R11-10 | **Given** the Reopen from AC-R5-13, **When** the CSP app notification to Shiv Net's Owner cannot be sent at 16:05, **Then** a record exists for that service task showing a "reopened" notification that could not be sent at 16:05. | R11e · T6 | Settled |

#### Kapture status and comment per action

Each comment is added by Wiom in Kapture at the moment of the action, and only for the actions listed here (an app notification that could not be sent gets no comment), with the Status and Sub Status shown. `{…}` is filled with the actual value; times are IST; durations are the current values of C-01, C-04 and C-05. A Kapture close before any Resolved mark (T15) adds nothing new: it works as today. Wherever a comment names the Technician, "and Technician {name}" is left out when no Technician is assigned. One comment per action that takes effect: a repeated tap that changes nothing (for example a second yes) adds none.

| # | Action | Status · Sub Status after it | Comment text |
|---|---|---|---|
| K1 | T1 · CSP or Technician marks Resolved | Pending · Resolved | Marked Resolved by {CSP / Technician} {name} at {time}. Wiom is now verifying the fix with the customer. The ticket closes when the customer confirms, or automatically after {C-04} or {C-05}. |
| K2 | T2 · positive Internet Verdict during the Initial Check | Pending · Resolved | Internet Verdict positive at {time}. First message sent to the customer at {time}, asking whether the internet is working. |
| K3 | T3 · C-01 runs out with no positive Internet Verdict | Pending · Resolved | No positive Internet Verdict within {C-01} of the Resolved mark. First message sent to the customer at {time}, asking whether the internet is working. |
| K4 | T13 · positive Internet Verdict after C-01, before C-04 runs out | Pending · Resolved | Internet Verdict positive at {time}, after the Initial Check. No new message sent to the customer. |
| K5 | T5 · customer says yes | Complete · Completed | Customer confirmed the internet is working at {time} (via app). Ticket closed — reason: Customer confirmed. CSP {CSP name} and Technician {name} notified. |
| K6 | T6 · customer says not working and reopens the previous ticket | Pending · Open | Customer said the internet is not working and reopened the previous ticket at {time} (via app). Ticket sent back to CSP {CSP name}; CSP {CSP name} and Technician {name} notified. |
| K7 | T6 · Wiom CX user records the customer asking to reopen, or sets Open in Kapture | Pending · Open | {Agent name} recorded at {time} that the customer said it is not working and asked to reopen it — or, when the agent set it Open: {Agent name} set the ticket to Open in Kapture at {time}. Either way: ticket reopened and sent back to CSP {CSP name}; CSP {CSP name} and Technician {name} notified. |
| K8 | T7 · auto closure at C-04 | Complete · Completed | No answer from the customer within {C-04} of the Resolved mark; Internet Verdict was positive at {time}. Ticket closed automatically — reason: Internet working, no response. No message sent to the customer. CSP {CSP name} and Technician {name} notified. |
| K9 | T8 · auto closure at C-05 | Complete · Completed | No answer from the customer within {C-05} of the Resolved mark, and no positive Internet Verdict within {C-04}. Ticket closed automatically — reason: Not verified. No message sent to the customer. CSP {CSP name} and Technician {name} notified. |
| K10 | T14 · Wiom CX user sets Completed or Closed during Pending Verification | Complete · Completed | Closed by {Agent name} in Kapture at {time} during verification — reason: Closed by the Wiom CX team. Customer sent the usual closure message. CSP {CSP name} and Technician {name} notified. |
| K11 | A Wiom CX user tries to reopen a Closed ticket in Kapture — blocked | Unchanged (Complete · Completed) | Reopen blocked at {time}: a closed ticket cannot be reopened. Raise a new ticket for the customer. |
| K12 | Customer says not working and chooses to raise a new ticket | Unchanged (Pending · Resolved) | Customer said the internet is not working at {time} and chose to raise a new ticket; directed to call Wiom. Ticket unchanged. |
| K13 | Customer taps नहीं and chooses neither option | Unchanged (Pending · Resolved) | Customer said the internet is not working at {time} but did not choose to reopen the ticket. Ticket unchanged. |
| K14 | Customer tries to reopen a Closed ticket in the chat — refused | Unchanged (Complete · Completed) | Customer tried to reopen the closed ticket at {time} (via app); refused, as a closed ticket cannot be reopened. |

In P1, Kapture already shows the Status and Sub Status in this table (R8a–c); P4 adds the comments.

---

## 4. Cross-cutting Acceptance Criteria

| AC | Given / When / Then | Verifies | Status |
|---|---|---|---|
| AC-R12-8 | **Given** CSP Om Sai Cable is not on the rollout list, **When** it marks an ISD service task Resolved at 13:47 and it closes as today, **Then** Kapture shows today's status and comments, with none of K1–K14, and no record of this spec's notifications or chat answers is made for it. | R12b · R12 MUST NOT · G8 · R8d · R11 | Settled |
| AC-GRD-6 | **Given** every service task that left Pending Verification in a day, **When** a Wiom Product user reads the recorded cause of each exit, **Then** each was the customer's yes, the customer's Reopen, a Wiom CX user recording the customer asking to reopen, a Wiom CX user's Open or close in Kapture, or an auto closure, and each has a matching Kapture comment. | G6 · R8d | Settled |

---

## 5. System Behaviour

Lifecycle: P1 §5, unchanged. This phase only records: a Kapture comment on every edge (table above) and a record of every notification and chat answer. Neither may change what happens (R11 MUST NOT).

---

## 6. Configurability

No parameters of its own; P1's apply.
