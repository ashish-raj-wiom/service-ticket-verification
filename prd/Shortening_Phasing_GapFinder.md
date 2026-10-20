# How the Service Task Verification PRD was shortened, phased and gap-checked

What was done to PRD v1.2 (signed off 15 Oct 2026) after Marut's review, 20 Oct 2026, in three steps. v1.2 itself is never edited.

| Step | Output | Part |
|---|---|---|
| 1. Shorten v1.2 by removing repetition | `Service_Task_Verification_PRD_v1.2-short.md` | A |
| 2. Split the shortened PRD into phases | Four phase PRDs in `phases/`: P1 Core loop · P2 Internet Verdict after the Initial Check · P3 Quality OS signals and M3 · P4 Records | B |
| 3. Run the product-gap prompt on every PRD, and decide every gap | Decisions applied to the shortened PRD, and from it to the phase PRDs | C |

The four phase PRDs are built and tested one by one, then tested together and shipped together, CSP by CSP through the rollout list (P1 §2).

---

## Part A — Shortening

Step 1 of 2 (shortening). Sizes include the small additions from Part C (20 Oct 2026), which are changes, not cuts. The signed-off `Service_Task_Verification_PRD.md` (v1.2, 15 Oct 2026) is untouched; the shortened copy is `Service_Task_Verification_PRD_v1.2-short.md`. The shortening itself changed no rule, guardrail, parameter or behaviour; only what is listed here was removed or merged. The product-gap decisions did change R4f, R5c, R5f and the Boundary; see that file.

| | v1.2 | v1.2-short |
|---|---|---|
| Characters | 91,710 | 85,741 (−7%) |
| Words | 16,797 | 15,876 |
| Terms in §1 | 25 | 16 |
| ACs | 133 | 137 (27 removed, 1 restored, 31 added; see §7 and Part C) |
| Sections | §1–§8 | §1–§6 |
| Rule obligations and guardrails with a test | 110 | 110 (none lost) |

### 1. Terminology: only new terms (Marut)

Checked against Wiom Operational Terminology and Wiom System Terminology (v0.7, fetched 20 Oct 2026). Neither file defines any of the 25 terms, so the cut follows Marut's test instead: keep only what this spec coins or gives a new meaning.

**Removed: existing words that this spec does not change (9):** Resolved, In Progress, Closed, ISD, RDNI, Resolution signal, M3, Kapture, Status / Sub Status (Kapture). ISD and RDNI stay spelled out in §2 Boundary.

**Kept: coined or redefined here (16):** Pending Verification, Initial Check, Positive verdict, No positive verdict, First message, Internet Verdict, First positive verdict, Reopen, Customer's No, Auto closure, Rollout list, Closure reason, M3 timer (now pauses), M3 cycle (new fixed cycles), CSP TAT (now summed over rounds), Within TAT (now judged at Closure on CSP time only).

Follow-up: none of the 9 removed terms is in the global files either. Adding them there (as the Restructure Notes also suggest) would let every PRD leave them out.

### 2. §7 Impacted Systems & References: removed (Marut)

The "copy: Figma files, §7" pointers now say "copy: Figma, header"; the Figma table in the header is kept. Two things lived only in §7 and are gone with it:
- **Connection Validation Service load:** 54 audit records per service task at default settings, about 316 at the fastest, and "engineering confirms Connection Validation Service takes this load before release". This is a release check, so it should move to the engineering handoff or the release checklist.
- **Today's behaviour per system** (for example `RS` from Kapture resolves, the 2-hour CC auto-complete, M3's rolling 15-day window). It is engineering context, not a rule.

§6 still names `SR_SLA_NO_INTERNET` / `SR_SLA_RECHARGE_DONE_NO_INTERNET` in C-15. That is a live-code reference of the same kind; it can go if C-15 just says "today's value, unchanged".

### 3. §8 Notes for System Capabilities: removed (Marut)

Every row restated a rule already in §3 (R1–R12), so nothing was lost.

### 4. Repeated ACs

Every AC was compared against every other. An AC was removed when another AC already asserts everything it asserts. Where the two shared the same Given and When but the removed one added a small detail, that detail was folded into the kept AC, whose Verifies column now also carries the removed one's. Group ACs (WF, FAIL, GRD, BV) were removed when an AC under a rule already tests the same case. Boundary tests on a limit (BV-1, BV-2, BV-7, BV-8, BV-9, BV-11, BV-12), race, config and duplicate-trigger tests were kept: they test an edge, not a repeat.

#### Removed (27)

| Removed | Repeats | What happened |
|---|---|---|
| AC-R1-3 | AC-R1-6 | Same check (CSP app shows only "Wiom will verify"); "Technician App as today" added to R1-6 |
| AC-R12-1 | AC-R1-1 | Same case; R1-1 now says Shiv Net is on the rollout list and verifies R12a |
| AC-R3-7 | AC-R1-4 | R1-4 already asserts no message at the mark; "including today's 'resolved' message" added |
| AC-R2-9 | AC-R2-1 + AC-R2-8 | Positive verdict inside the Initial Check, at another minute; the ask schedule is R2-8 |
| AC-R3-3 | AC-R3-2 | Same Given and When; the two channels added to R3-2 |
| AC-R4-9 | AC-R2-4 | R2-4 already asserted the 16 Oct auto closure; the closure reason added |
| AC-R4-4 | AC-R4-8 + AC-R2-8 + AC-R4-3 | "Still in Pending Verification at C-04" is R4-8; "no asks after C-04" is R2-8; "closed at C-05" is R4-3 |
| AC-BV-13 | AC-R4-8 | Same moment at second precision; R4-8 now uses 13:47:00 |
| AC-R4-5 | AC-R4-3 | Same service task; "no message" added to R4-3 |
| AC-FAIL-4 | AC-R4-3 | Same Given, timer, reason and "no message" |
| AC-FAIL-3 | AC-R4-2 | C-04 auto closure with no message; "no message" added to R4-2 |
| AC-FAIL-1 | AC-R2-3 + AC-R3-2 | C-01 runs out: No positive verdict, first message sent |
| AC-R4-6 | AC-R4-10 | Same Given and When; "CSP app shows it Closed" added to R4-10 |
| AC-R4-12 | AC-R4-11 | Same Kapture close; the customer-message assertion added to R4-11 |
| AC-BV-4 | AC-R4-1 + AC-RACE-2 | A yes before the timer closes "Customer confirmed" |
| AC-R5-8 | AC-R5-13 | Same Reopen and Technician; the notifications and "nothing says the customer reopened it" added to R5-13 |
| AC-R5-9 | AC-R5-5 + AC-R10-3 + AC-R10-7 | Night Reopen: TAT wait is R5-5 (notifications added); M3 resume and night running are R10-3 and R10-7 |
| AC-R6-2 | AC-R6-9 | "Not verified" never signalled as fixed; R6-9 already asserts it |
| AC-R8-7 | AC-GRD-1 + AC-R9-1 | Kapture reopen of a Closed task refused (now in GRD-1); new fresh task (R9-1, now verifies R8f) |
| AC-R9-3 | AC-GRD-1 | "Any actor tries to move it" added to GRD-1 |
| AC-BV-5 | AC-R9-1 | Fresh new task after Closure; CSP TAT and M3 from zero, own within-TAT, added to R9-1 |
| AC-R11-1 | AC-R11-2 | Same push; the "first message" type and sent time added to R11-2 |
| AC-WF-1 | AC-R2-1, AC-R3-1, AC-R4-1, AC-R6-1 | Every step is tested under its rule. AC-WF-2 is kept: only it tests C-04 counting from the second mark |
| AC-GRD-4 | AC-R6-1 | No signal between mark and Closure. G4 now anchors R6-1 and DUP-1 |
| AC-GRD-5 | AC-R7-1 | TAT sums In Progress rounds, leaves Pending Verification out. G5 anchors R1-2, R6-4, R6-5, R7-1, R10-2 |
| AC-GRD-7 | AC-R10-2 | M3 timer paused in Pending Verification, task not counted |
| AC-GRD-9 | AC-R12-2 | Off-list CSP closes at the mark as today. G8 anchors R12-2, R12-3, R12-5 |

#### Kept, with the repeated clause taken out (4)

The user's own example, "A CX agent closes the task", had five ACs. Now each tests one thing: **R4-11** closure, reason, notifications and customer message · **R8-13** the Sub Status Closed variant · **R8-6** the Kapture status and K10 comment only · **R6-3** the Quality OS signal only.

| AC | Clause removed | Where it is tested |
|---|---|---|
| AC-R8-6 | Closed with "Closed by the Wiom CX team" | AC-R4-11 |
| AC-R8-14 | Kapture shows Pending / Open after a Reopen | AC-R8-3 |
| AC-R8-16 | Kapture shows Complete / Completed at Closure | AC-R8-2 |
| AC-R11-3 | The service task is Reopened | AC-R5-1 |

References updated: G1, G4, G5 and G8 anchors; AC-R11-7 and AC-R11-9 now point to AC-R5-13 instead of AC-R5-8. A check confirmed no removed AC is referenced anywhere in the shortened PRD.

#### Guardrail tests after the cut

G4, G5 and G8 no longer have a GRD-* group AC: their population-level GRD ACs repeated a rule AC, so they now anchor to that AC. If the skill's lint insists on one GRD AC per guardrail, this is the deliberate exception, following the feedback "point to that one instead of writing a new one".

### 5. §5 "Not possible from here": 17 lines to 4

Each removed line restated something a rule already says. The 4 kept are timer facts no rule states: the verification clocks do not run In Progress; each Pending Verification stage has only its own auto-closure timer; C-04 and C-05 cannot expire during the Initial Check; the Initial Check cannot end twice.

| Removed line | Already stated by |
|---|---|
| Initial Check × customer yes | R4a (a yes comes after the first message) · R5c |
| In Progress × customer yes or no | §2 Boundary · G7 |
| In Progress × plan expiry | §2 Boundary |
| Pending Verification × CSP marks Resolved | R1 MUST NOT |
| Pending Verification × other customer contact | R5 MUST NOT |
| Pending Verification × नहीं with the new-task choice, or no choice | R5g · R5h |
| Pending Verification × other Kapture change, or a status Wiom set | R8 MUST NOT |
| Positive verdict × a later verdict that is not positive | R2 MUST NOT |
| No positive verdict × a positive verdict after C-04 | R2d · R4d · R2 MUST NOT |
| In Progress × "not working" | G7 |
| In Progress × M3 timer reaches C-08 again | R10 MUST NOT |
| Pending Verification × M3 timer reaches C-08 | R10b |
| Closed × every trigger except T11 | G1 · R8 MUST NOT · R9b · R10e |

### 6. Fix found while splitting (20 Oct 2026)

AC-WF-2 began "Given the same service task", which pointed at AC-WF-1, removed in §4. Its Given now names the service task itself: "an ISD service task In Progress from 11:20 on 14 Oct, marked Resolved by the Technician at 13:47". No assertion changed.

### 7. Blast radius: one cut reversed, three tests added (20 Oct 2026)

PM: a CSP off the rollout list must never enter the verification flow; its world works exactly as today. A must-have, to keep the blast radius small. This reverses one cut and adds tests; it is logged here because it changes the AC count.

- **AC-GRD-9 restored** (cut in §4 as a repeat of AC-R12-2). It is the only whole-day check that off-list CSPs see no change, so it is back, widened to every system: none entered Pending Verification, every one closed at its mark, no verification message, today's Kapture statuses, today's CSP and Technician notifications. G8 anchors it again.
- **Added, one off-list test per system a later phase touches:** AC-R12-6 (chat, P1), AC-R12-7 (Internet Verdict asks, P2), AC-R12-8 (Kapture comments and tracking records, P4). AC-R12-2 (closure, customer message, Quality OS) and AC-R12-5 (M3) already covered the rest.

### 7a. Gap review round 2 (20 Oct 2026)

Not a cut: the gap finder was run again on the shortened PRD and the phase PRDs, and the PM's decisions were applied. 20 ACs added and 3 edited, R2d, R3, R4h, R5b, R5g, R6b, R6c, R6e, R9c, R10, R11e, R12d and the K table changed. Every change is listed in Part C, round 2. It is noted here because it moves the size table.

### 8. Why it is still 86k characters

Shortening alone took v1.2 from 91.7k to 72.6k characters (−20%); the tests added for the blast radius, the gap review and the Netbox swap (Part C) bring it to 86k (−7%). The 27 removed ACs were the repeats. What remains is one test per obligation, plus edge, race and config tests. Most of the remaining size is the rule rows (the R4, R5 and R8 MUST cells are long) and the K1–K14 Kapture comment table. Shortening those would change wording that was signed off, so it was left for a separate decision.

---

## Part B — Phasing

Step 2 of 2 (phases), 20 Oct 2026. Split from `Service_Task_Verification_PRD_v1.2-short.md` (Part A) into four PRDs in `phases/`. Neither v1.2 nor v1.2-short is edited by the split. Built by script (scratchpad `split_prd4.js`), so the PRDs can be rebuilt after any change to v1.2-short. An earlier phasing proposal was not used and has been deleted (PM, 20 Oct).

### The model

- **Marut:** too long to implement and test in one go, so roll it out in phases.
- **PM:** the whole feature must be tested together for completeness.
- **So:** four PRDs, one per phase. Each phase is built and tested behind the rollout list while it is empty. All four are then tested together on one build, and released once, with CSPs added to the rollout list one by one. Nothing goes live between phases, so no in-between behaviour, no new ACs and no change to any rule were needed.

### The four PRDs

| Phase | File | Builds | Needs | ACs | Characters |
|---|---|---|---|---|---|
| P1 Core loop | `phases/STV_P1_Core_Loop_PRD.md` | Pending Verification, Internet Verdict at the mark and through the Initial Check, first message, yes, the three closure paths and reasons, the customer's reopen from the chat and a Wiom CX user's reopen, Kapture close, CSP TAT, Closed is final, CSP and Technician notifications, Kapture statuses (never acting on a status Wiom set), rollout list | — | 78 | 59,381 |
| P2 Internet Verdict after the Initial Check | `phases/STV_P2_Internet_Verdict_After_Initial_Check_PRD.md` | Asks every C-14 until C-04; a late positive verdict; stopping at C-04 (R2c, R2d after the Initial Check, R3e, R4d) | P1 | 10 | 9,604 |
| P3 Quality OS | `phases/STV_P3_Quality_OS_Signals_And_M3_PRD.md` | Signals only at Closure, with reason and within TAT; M3 pause, resume, fixed cycles, pass mark (R6, R10) | P1 | 24 | 16,251 |
| P4 Records | `phases/STV_P4_Records_PRD.md` | Kapture comments K1–K14 (R8d); notification and chat-answer records (R11) | P1 | 25 | 17,743 |
| **Total** | | | | **137** | |

Every one of the 137 ACs is in exactly one PRD (checked by the script). P2 and P3 need only P1; P4 needs P1, and P2 for one comment (K4). They can be built in parallel.

**Why the chat reopen is in P1 (PM, 20 Oct):** it is one tap on a path P1 already builds (the Reopen), and the customer's own reopen is the core of the objective. The first split had it as its own phase; merging it also removed the gap where three P1 behaviours (R5b, R5e, R5f) were first tested at a later gate.

**Why the Kapture status guard (R8g, R8 MUST NOT) is in P1, not with the comments:** today an `RS` coming from Kapture resolves a task and `OP` reopens it, so without the guard Wiom's own status writes could close or reopen tasks during the P1 tests.

### How the source was divided

| Source part | Where it went |
|---|---|
| Header, Figma table | Every PRD; title, version and a Phase row per PRD |
| §1 terms | P1 has the feature's terms; P3 adds Within TAT, M3 timer, M3 cycle; the others use P1's |
| §2 Objective, Boundary, push-copy line | P1 in full; each other PRD has its own one-line objective |
| Only CSPs on the rollout list | Opens §2 in every PRD, naming that phase's off-list test (blast radius, below) |
| Phases and test gates | P1 §2 in full (table of the four PRDs, three test gates); a short version in each other PRD |
| Guardrails | P1: G1, G2, G3, G5, G6, G7, G8; P3: G4, G5; P2 and P4 name the ones they must keep |
| Rules split by letter | R2: P1 (a, b, d at the mark and in the Initial Check) · P2 (c, d after it). R3: P1 (a–d) · P2 (e). R4: P1 (a–c, e–h) · P2 (d). R8: P1 (a–c, e–g) · P4 (d). Letters are kept, so every cross-reference still holds |
| Whole rules | P1: R1, R5, R7, R9, R12 · P3: R6, R10 · P4: R11 |
| MUST NOT split | R2 "count a positive Internet Verdict after C-04" → P2. R8 "show the CSP what the Kapture comments say" → P4. P2's R4 points to P1's MUST NOT |
| R2d | Rewritten in two halves: P1 asks at the mark and every C-13 in the Initial Check; P2 asks every C-14 after it. Same meaning, split at the Initial Check |
| K1–K14 table | P4; P1's R8a–c already set the Status and Sub Status columns |
| §5 diagram, Not possible, Precedence | P1 keeps the whole diagram, noting T13 is P2 and T12 is P3; P2 and P3 describe the edges they build; P3 takes the M3 precedence line |
| §6 parameters | P1: C-01, C-04, C-05, C-06, C-12, C-13 and the first interaction note · P2: C-14 · P3: C-08, C-09, C-10, C-15 and the M3 interaction note |

### How an AC got its phase

The earliest phase at which everything it checks exists:
1. the latest phase of anything in its Verifies column;
2. P2 if it uses an Internet Verdict after the Initial Check;
3. at least the phase of any AC its Given builds on. A reference in brackets is only a pointer and does not count.

Corrected by hand: AC-R2-8 → P2 (47 hourly asks); AC-R2-7 → P2 (its verdict is asked at 15:30, after the Initial Check).

### Assignment (137 ACs)

| Phase | ACs | IDs |
|---|---|---|
| P1 | 78 | R1-1 · R1-2 · R1-4 · R1-6 · R1-5 · R2-1 · R2-6 · R2-2 · R2-3 · R2-7 · R3-1 · R3-2 · R3-4 · R4-1 · R4-2 · R4-3 · R4-10 · R4-11 · R4-13 · R4-7 · R5-1 · R5-2 · R5-13 · R5-3 · R5-4 · R5-7 · R5-5 · R5-6 · R5-10 · R5-11 · R7-1 · R7-2 · R8-1 · R8-2 · R8-3 · R8-8 · R8-9 · R8-10 · R8-13 · R8-14 · R8-15 · R9-1 · R9-2 · R9-4 · R12-2 · R12-3 · R12-4 · WF-2 · REG-2 · GRD-1 · GRD-2 · GRD-3 · GRD-8 · BV-1 · RACE-1 · RACE-2 · RACE-5 · CFG-1 · CFG-2 · CFG-3 · CFG-5 · BV-11 · DUP-2 · DUP-3 · GRD-9 · R12-6 · R3-8 · R12-9 · R5-14 · R5-15 · R5-16 · CFG-7 · R1-9 · R1-10 · R2-11 · R3-9 · R5-17 · R12-10 |
| P2 | 10 | R2-4 · R2-8 · R3-6 · R4-8 · BV-2 · CFG-6 · BV-12 · R12-7 · R2-10 · BV-14 |
| P3 | 24 | R6-1 · R6-3 · R6-4 · R6-5 · R6-6 · R6-7 · R6-8 · R6-9 · R10-1 · R10-2 · R10-3 · R10-4 · R10-5 · R10-6 · R10-7 · R12-5 · BV-7 · BV-8 · BV-9 · RACE-3 · DUP-1 · R10-8 · R10-9 · R6-11 |
| P4 | 25 | R8-4 · R8-5 · R8-6 · R8-11 · R8-12 · R8-16 · R11-2 · R11-3 · R11-4 · R11-5 · R11-7 · R11-8 · R11-9 · GRD-6 · R12-8 · R8-18 · R8-19 · R8-20 · R8-21 · R8-22 · R8-23 · R8-24 · R8-25 · R11-10 · R8-26 |

Machine-readable: `notes/phase-assignment.json`.

### Blast radius (20 Oct 2026)

Every phase PRD opens §2 with **Only CSPs on the rollout list**: what that phase builds applies only to CSPs on the list, and for any other CSP that part of the world works exactly as today (R12, G8), each with its off-list test. P1 states it for the whole feature, the chat included. Four ACs came with it (Part A §7): AC-GRD-9 and AC-R12-6 (P1), AC-R12-7 (P2), AC-R12-8 (P4).

### Changes to the split

| Date | Change |
|---|---|
| 20 Oct 2026 | First split: five PRDs (P1 core, P2 customer reopens from the chat, P3 verdict after the Initial Check, P4 Quality OS, P5 records) |
| 20 Oct 2026 | Chat reopen merged into P1 (PM: "it's just a tap which reopens the ticket"). Four PRDs; the others renumbered (P3→P2, P4→P3, P5→P4). The live page and the publish script follow |
| 20 Oct 2026 | Gap review round 2: 20 ACs added and placed (P1 +6, P2 +2, P3 +2, P4 +10); AC-R8-8 and AC-R2-7 moved to P1; P4 now also needs P2 for K4 (Part C) |
| 20 Oct 2026 | Netbox swap counts as the Resolved mark: 8 ACs added (P1 +6, P3 +1, P4 +1); R2e added to P1 (Part C) |

### Fixes made while splitting

- AC-WF-2's Given pointed at a removed AC (WF-1). Fixed in v1.2-short (Part A §6).

### Open

- P1 is about 59k characters, well over the 20k Marut used; it holds the core and 78 ACs. Shortening it further means cutting signed-off rule wording or ACs that are the only test of an obligation.
- Engineering to confirm the build order and whether P2–P4 run in parallel.
- Rebuild with `split_prd4.js` after any change to v1.2-short, then update this part.

---

## Part C — Product gaps

### The prompt

The same prompt found the first gaps (on v1.2, from tech) and was then run on the shortened PRD and on each phase PRD, one reviewer per document:

> You are a Product-Gap Finder. Your ONLY job is to surface genuine PRODUCT / SPEC / SCOPE gaps in the issue - things a product owner (not an engineer) must decide. You are NOT planning implementation and you do NOT raise engineering decisions.
> SURFACE (product_gaps) only things like:
> - whether a change is in scope at all (e.g. 'is an app change in scope given the stated boundary?');
> - mutually-exclusive product behaviors where picking wrong has user/business consequences and the spec gives no basis (e.g. 'does "cannot be identified" cover only hard errors, or also the legitimate "no resolution found" case?');
> - product data-contract questions with user-visible meaning (e.g. single- vs multi-select for rating reason; verbatim label vs stable code when counting is required);
> - missing acceptance criteria, conflicting requirements, or scope boundaries that are genuinely a product call.
>
> DO NOT surface (these are ENGINEERING decisions made later, NEVER gaps): API endpoint paths/methods, request/response payload & DTO shapes, transport (sync vs async/queue), retry/idempotency/timeout values, storage modeling (columns vs new table, types, length limits), serialization/parsing formats, naming, query-vs-structural isolation. If an item is a mix, surface ONLY the product part and leave the engineering part for the plan stage.
> For each product_gap give: title, whats_unclear (the product question), why_it_matters (user/business impact). If there are genuinely no product gaps, return an empty list. Return ONLY the structured object.

Product gaps found with the prompt below, and decided by Ashish Raj (PM). Applied to `Service_Task_Verification_PRD_v1.2-short.md` and from there to the phase PRDs; the signed-off v1.2 is not edited. These are changes, not cuts; cuts are in Part A.

### Round 1 — questions from tech and the customer app team (20 Oct 2026)

| # | Raised by | Question | Decision | Change in the PRD |
|---|---|---|---|---|
| T1 | Tech | Which Technician gets the Reopen and Closure notifications when several worked the task? | Only the last assignee; earlier Technicians on the task are not notified. | R4f and R5f end with that sentence. AC-R5-13 now has an earlier Technician (Suresh) who gets nothing. |
| T2 / CA5 | Tech, customer app | Apology coupon across Reopen rounds; does its message say "resolved" before Wiom asks? | This PRD is about verification only. The coupon follows today's mechanism unchanged; compensation is redone in a later PRD. | §2 Boundary: "the apology coupon, which follows today's mechanism unchanged; this spec does not change how or when it is issued." The old line ("still goes out at the Resolved mark as today") is removed, since it described the mechanism. |
| T3 | Tech | A CX agent's plain "not working" reopens without the two-step the chat needs | A Wiom CX user records a "not working" when the customer asks for this service task to be reopened. | R5c reworded. AC-R5-3, AC-R5-4, AC-R5-7, AC-GRD-6, K7 and the T6 edge in §5 now use the same wording; no "plain no" is left in the PRD. |
| CA1 | Customer app | Which chat buttons and APIs handle yes, नहीं, reopen and new task; reused or new? | Engineering, not the PRD. | None. Existing pieces engineering can start from: `POST /complaint/reopenTicket` (`booking-service-java`, `ComplaintCallbackController.java:47`) and the `complaint_resolved` chat workflow (`CustomerWorkflows.java:84`). |
| CA2 | Customer app | Customer taps नहीं and picks no choice: what does the chat say? | No change. A Reopen happens only when the customer taps नहीं and then the reopen choice (copy as set in Figma). Otherwise the task carries on and auto closes; the first message already told the customer the issue is resolved. | None: R5h already says it. |
| CA3 | Customer app | What the customer sees while Wiom checks (up to 72 h) | No extra message. Wiom has already told the customer the issue is resolved (first message). After Closure the customer must not be able to reopen it. | None: R3, R4e, R9c and G1 already say it. |
| CA4 | Customer app | No message on auto closure, even "Not verified" (C2 / guardrails G9 conflict) | Confirmed: no message on any auto closure. | None: R4e already says it. The C2 conflict flagged in `guardrails.md` (G9) is accepted by the PM. |

### Round 2 — gap finder run on the shortened PRD and the four phase PRDs (20 Oct 2026)

The same gap-finder prompt that found the first gaps was run on the shortened PRD and on P1–P4, one reviewer per document. 36 findings came back; after merging overlaps and checking each against the PRDs, 5 were document defects (fixed without a decision), 14 needed a PM decision, 3 were already decided (SMS/IVR out of scope; plan expiry changes nothing; no message on auto closure) and 2 were not product gaps (a wrong AC count; where Product reads exit causes is tooling).

#### Document defects fixed

| # | Defect | Fix |
|---|---|---|
| A1 | AC-R8-12 expected "CSP Shiv Net and the assigned Technician notified" but K8 named no one; comments named people inconsistently | Every comment names the CSP and, when assigned, the Technician; the table note says the Technician is left out when none is assigned |
| A2 | K4 (late positive verdict, T13) is in P4, but T13 is built in P2 and P4 said it needed only P1 | P4 now needs P1, and P2 for K4 |
| A3 | The Kapture close before any mark (T15) had no P1 test: its only AC, AC-R8-8, also checked the M3 timer (P3) | AC-R8-8 keeps the "as today" close and moves to P1; the M3 part is the new AC-R10-8 (P3) |
| A4 | K7 said "recorded the customer's No" also when the agent only set Open | K7 has a wording for each of its two actions |
| A5 | C-08 and C-10 were not marked as today's values | Both marked "unchanged by this spec" (today's Quality OS values) |

#### PM decisions

| # | Question | Decision (PM) | Change in the PRD |
|---|---|---|---|
| B1 | Which closure reasons count as "fixed" for Quality OS? | All of them. Quality OS needs only whether the service task was within TAT; how it closed does not matter to it. | R6b: every Closure is signalled as fixed; R6c: including "Closed by the Wiom CX team" and "Not verified". AC-R6-9 now expects "fixed" |
| B2 | A tap on an earlier round's first message | Only the current round's message takes answers; an old one changes nothing and the chat says the issue is being worked on | New R3f; AC-R3-8 |
| B3 | CSP removed from the list between rounds (R12a vs R12d) | R12d wins: once on this spec, every round stays on it until Closed | R12d widened; AC-R12-9 |
| B4 | M3 method for the cycle a CSP joins or leaves the list in | The method that applied on the cycle's first day judges the whole cycle | New R10g; AC-R10-9 |
| B5 | Customer picks "नई शिकायत दर्ज करें" and calls Wiom CX | The agent may open a new service task; the one in Pending Verification stays there | R5g widened; AC-R5-14 |
| B6 | After Closure, how is a new service task raised? | The way it is today; no change | R9c and AC-R9-4: "a new service task is raised the way it is today" |
| B7 | Customer tells a CX agent on a call that it works | A Kapture close is "Closed by the Wiom CX team", whatever the customer said | R4h says so |
| B8 | Do parameter changes apply to tasks already waiting? | No: each round keeps the values in force at its own Resolved mark | §6 line; AC-CFG-7 |
| B9 | Reopen after the task was moved to another CSP | The CSP that holds it at the Reopen | R5b; AC-R5-15 |
| B10 | Reopen when the last Technician is deactivated | Back to the CSP app, unassigned | R5b; AC-R5-16 |
| B11 | Within TAT for a Kapture close before any mark (T15) | Today's signal (R6d); the CSP-time judgement is for Closures from Pending Verification | R6e narrowed |
| B12 | When do the Internet Verdict asks stop; a verdict at the instant C-04 runs out | Stop at the round's first positive verdict, the customer's answer, a Reopen, the Closure, or C-04; a verdict at the same instant as C-04 counts as before it | R2d, R2c; AC-R2-7 rewritten (now P1); AC-R2-10, AC-BV-14 |
| B13 | Should Quality OS learn how many Reopens a task had? | No: do not alter the signal to Quality OS | None |
| B14 | Comments, records and tests that were missing | K13 (नहीं with no choice) and K14 (customer reopen refused after Closure); one comment per action that takes effect; failed CSP and Technician notifications are recorded; tests for K4, K7, K11, K12, K13, K14, the R8 MUST NOT and duplicate taps | K13, K14, table note, R11e; AC-R8-18 to AC-R8-25, AC-R11-10 |

20 ACs added (P1: AC-R3-8, AC-R5-14, AC-R5-15, AC-R5-16, AC-R12-9, AC-CFG-7 · P2: AC-R2-10, AC-BV-14 · P3: AC-R10-8, AC-R10-9 · P4: AC-R8-18 to AC-R8-25, AC-R11-10); AC-R2-7 rewritten; AC-R8-8, AC-R8-8 and AC-R6-9 edited. Total 129.


### Netbox swap as a Resolved mark (20 Oct 2026)

Found by a code review after approval: today a committed Netbox swap resolves and closes the service task on its own ("swap-as-closure"), with no Resolved mark, recorded as system verified without asking Connection Validation Service, and with the Quality OS signal at the swap. About 30 swaps a day (about 1.7% of service tasks created, 6–19 Oct). None of the PRDs mentioned it, so for CSPs on the rollout list a swap would have skipped verification.

PM: a committed swap should go to Pending Verification like any Resolved mark. Same Initial Check (no swap-specific timer); the Kapture comment names whoever confirmed the swap.

| Where | Change |
|---|---|
| §1 | New term: Netbox swap |
| §2 Boundary | The swap itself (start, run, confirm, fail) is unchanged; this spec sets only what happens at commit |
| G2 | "…or committed a Netbox swap on it" |
| R1 | New (e): a committed swap is the Resolved mark; a failed swap is not. MUST NOT: close at the swap, record it as verified without the Internet Verdict, start a swap in Pending Verification |
| R2 | New (e): after a swap, ask for the Internet Verdict of the Netbox swapped in |
| R12b | Off-list CSPs: a swap closes the task as today |
| §5 | T1 includes a committed swap; Not possible: Pending Verification × start of a swap |
| K1 | Swap wording: "Netbox swapped from {old} to {new} by {CSP / Technician} {name} at {time}" |
| ACs | AC-R1-5 edited (no swap offered in Pending Verification). New: AC-R1-9, AC-R1-10, AC-R2-11, AC-R3-9, AC-R5-17, AC-R12-10 (P1) · AC-R6-11 (P3) · AC-R8-26 (P4) |

Code reference for engineering (not in the PRD): `DeviceSwapServiceImpl.handleSwapCommitted` → `ResolutionFinalizer.finalizeResolvedSystemVerified(…, "PASS")`; a swap starts only for an ASSIGNED or ESCALATED complaint; `SWAP_FAILED` leaves it open.

### Review feedback after approval (ongoing)

Validation of the phase PRDs against Dhruv's latest docs (Harsh posts each finding on the ticket). Wording only; no rule, AC or behaviour changes. Each fix is made in the shortened PRD or the build script, so a rebuild keeps it.

| Date | PRD | Was | Now |
|---|---|---|---|
| 20 Oct 2026 | P1 §2 "Only CSPs on the rollout list" | its service tasks, customers | its service tasks, connections |
| 20 Oct 2026 | P1 §1 Internet Verdict (and the shortened PRD) | today from router signals | today from Netbox signals |
| 20 Oct 2026 | P2 §2 Objective | a router that comes back late | a Netbox that comes back online late |

After these, no PRD uses "router".

### Blast radius (20 Oct 2026)

PM: this is a CSP-level rollout. A CSP not on the rollout list must never enter the verification flow; its world works exactly as today. A must-have, to keep the blast radius small. Already the rule (R12, G8, zero tolerance); now stated in every phase PRD and tested in every system: AC-R12-2, AC-R12-5, AC-R12-6, AC-R12-7, AC-R12-8 and the restored whole-day AC-GRD-9 (see Part A §7).

### Today's apology coupon mechanism (checked 20 Oct 2026, for T2)

Not written into the PRD (decision T2); recorded here so the decision rests on facts.

- **Trigger.** When a service task is resolved, `ticket-service-java` puts a `CUSTOMER_TICKET_NOTIF` message with action `TICKET_RESOLVED` on the customer queue (`TicketServiceImpl.java:864`). `customer-java` handles it (`CustomerFunctions.java:977`): for a SERVICE ticket it runs the coupon engine, `processTicketBenefits` (`TicketService.java:86`), and sends today's "resolved" push, or SMS if the push fails.
- **One per ticket.** The engine skips a ticket that already has a coupon (`isCouponAlreadyAllocated`, by ticket id). A second Resolved mark on the same ticket gets no second coupon, unless the first round got none.
- **Size and limits.** The size comes from the hours between raising and resolving, and whether it is the only service ticket in the customer's plan cycle (`t_ticket_resolution_bucket`). The number of apology coupons per plan cycle is capped (`shouldAssignCoupon`). It sends its own push (`APOLOGY_..._NOTIFICATION`).
- **Today, it rarely fires.** Since April 2026 almost no coupons go out (about 0.5–2% of resolved service tickets), because `FetchLastPlan` sorts two epoch formats together (see the apology coupon study, 6 Oct 2026).

**For engineering:** the coupon, its own push and today's "resolved" message ride the same `TICKET_RESOLVED` event. R3 MUST NOT stops today's "resolved" message at the mark for CSPs on the rollout list. Keeping the coupon "unchanged" therefore needs a decision on which event still runs the engine, and whether the coupon's own push goes out at the mark. That is an implementation question inside "today's mechanism, unchanged"; it does not reopen T2.
