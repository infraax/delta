# RWS 2.0 — Checks

Run the sections that apply. Each line is a yes/no question. A "no" is a finding: report it with the rule ID and the fix. Do not silently fix and move on; record what was wrong.

## A. Attachment (any governed project)

- [ ] `rws/` directory or Apparatus project exists. (§13.1)
- [ ] Exactly one `continuity`, exactly one `execution`, at least one `policy` are bound. (R-02)
- [ ] Every agent/service that writes has its own principal and holds only `execution` or `adviser`. (R-08)
- [ ] No policy text, schema, type, queue or API is named after a person, title, tool or Dutch institution. (R-06, P-10)
- [ ] `rws/asset.md` names what must keep working. (§13.3)
- [ ] `rws/reviews.md` lists every V-01 review with a next date. (V-01)
- [ ] `rws/sources.md` lists one authoritative source per shared field. (A-07)
- [ ] Only one receipt chain exists: Apparatus, or the X-11 buffer, never both. (C-02, X-11)

## B. Keep

- [ ] An active agreement covers every asset with keep work. (K-01)
- [ ] The agreement was signed under all three roles. (K-01)
- [ ] A performance floor exists with indicators under the five criteria, or a reason per missing criterion. (K-03, T-02)
- [ ] The current programme has firm / planned / outlook bands and a horizon ≥ the project minimum. (K-02, V-06)
- [ ] The programme has an explicit `displaced` list (empty only with a signer who checked). (K-09)
- [ ] Every item in `executing` is in the firm band, or carries `urgent: safety` with a displacement event. (K-06, K-08)
- [ ] No keep item has a gate receipt. (K-06)
- [ ] No keep item with `adds_function: true` is executing. (K-07)
- [ ] The keep envelope has `overprogramming: false` and no outgoing moves to build. (K-04)
- [ ] Every closed item updated the condition record. (K-06, K-11)
- [ ] If a disruption budget exists, the last period's `disruption_reported` event exists. (K-10, T-06)

## C. Build

- [ ] Every build item has one `lead`. (O-01)
- [ ] Every state change has a gate receipt of the right type. No phase skipped. (B-03, B-05)
- [ ] Every gate names signers holding the roles in B-02. (B-02)
- [ ] Every gate has at least one evidence Artefact that is not only agent output or a replica. (B-02, C-05)
- [ ] G1: start document includes lifecycle maintenance cost change and funding sight ≥ threshold. (B-08, M-06)
- [ ] G2: options include do-nothing and minimal; 100% means available for the preferred option. (B-02, B-08)
- [ ] G3: operating and maintenance plan and named operator present; reserve-capacity decision recorded. (B-02, B-13)
- [ ] No code, contract, deployment or purchase for `realising` before G3 `go`. (B-05, B-07)
- [ ] Every external act references a gate receipt. (B-07)
- [ ] Every no-go/stop released its means. (B-04, M-05)
- [ ] No exploration older than its review date without a G2 receipt. (B-04)
- [ ] Overruns: none, or `envelope_overrun` recorded and item returned to G3. (B-09)
- [ ] G4: signed by the handover mandate holder, not by the discharged executor; keep agreement named. (B-10, B-14.7)
- [ ] Small items inside a programme stay within the programme's admission rule. (B-12)

## D. Means

- [ ] Every envelope has a queue, ceiling, period, carry-over rule, overprogramming flag. (O-09)
- [ ] Bucket sums ≤ ceiling (build: except declared overprogramming). (O-09, M-09)
- [ ] No spend recorded against `reserved`. (M-02)
- [ ] Every `reserved → committed` references a gate or agreement. (M-05)
- [ ] No `committed → bound` for a build item before G3. (M-05)
- [ ] No `bound → reserved`. (M-10)
- [ ] No keep↔build move without a `continuity` receipt. (M-08, M-11)
- [ ] Timing shifts keep the multi-period total unchanged. (M-07)
- [ ] Money stored as integers with a unit. (X-04)

## E. Advice, policy, replica

- [ ] No receipt's `authority_ref` points to Advice or a replica. (A-01, A-08)
- [ ] Every `policy` receipt that adopts Advice has `advice_response` for each item. (A-03)
- [ ] No `policy` or `gate` receipt is signed by an agent principal. (A-05, R-08)
- [ ] Every evaluator finding is answered by its `respond_by`, or `advice_unanswered` is recorded. (A-06)
- [ ] Every dashboard, overview, index, cache, generated doc and README status table is marked `replica: true` with sources and generation time. (A-08)
- [ ] No local patch of an authoritative field; `report_back` used instead. (A-07)
- [ ] Every adopted ranking references an adopted framework. (A-09)

## F. Canon

- [ ] No Artefact or receipt edited or deleted in place. (C-01)
- [ ] Every ingest states outcome, purpose, digest, evidence tag. (C-03, C-05)
- [ ] Every `Restricted` object has provenance. (C-04)
- [ ] New restricted categories were introduced by a `continuity` policy receipt. (C-04)
- [ ] Historical imports keep `occurred_at`. (C-07)
- [ ] Searches that came up empty are recorded as `not_found`. (C-08)
- [ ] Receipt payloads serialise with sorted keys. (X-04)

## G. Failure and review

- [ ] Every F-02 event has `impact` and `next_decision {role, by}`. (F-01)
- [ ] No dated target has passed without `target_met` or `target_dropped`. (F-03)
- [ ] Open failures with `next_decision.by` in the past are listed in the next review. (F-05)
- [ ] Every out-of-cycle review names a V-04 trigger with an existing ref. (V-04)
- [ ] No active agreement was superseded mid-period without an allowed trigger. (V-05)
- [ ] Strategic re-weighing produced all six step artefacts before adoption. (V-02)
- [ ] Indicators with `steering: false` were reviewed (start steering or stop measuring). (T-03)
- [ ] The constraint register covers every asset in agreement scope. (T-04)
- [ ] Last scheduled restore test is recorded and passed, or `restore_failed` is open. (T-07)

## H. Agents, tools, MCP

- [ ] Each agent session writes under its own principal. (X-10)
- [ ] Agent outputs are stored as Advice with `producer_kind`. (A-05)
- [ ] MCP/tool surfaces cannot sign gates, adopt policy, move means, or write a log outside the project chain. (R-08, C-02)
- [ ] During an escalation, the coordinator signed no gate, means or policy receipt. (R-10)

## I. Refuse-list sweep (before finishing any task)

- [ ] I did not accept a vision/roadmap document as a gate.
- [ ] I did not put maintenance through build gates, or new function through keep.
- [ ] I did not create a second log or edit a decision record in place.
- [ ] I did not drop or move a dated target silently.
- [ ] I did not cite a dashboard/overview as authority for spend or transition.
- [ ] I did not adopt my own output.
- [ ] I did not change a rule, floor or agreement outside a scheduled review or named trigger.
- [ ] I did not name anything after a Dutch institution.
