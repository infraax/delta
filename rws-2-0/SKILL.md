---
name: rws-2-0
description: Apply RWS 2.0 operating rules to any project or codebase. Use when starting a project, designing architecture, adding agents or tools, planning keep versus build work, writing gates, ledgers, ingest paths, MCP surfaces, or when the user mentions RWS, Apparatus, Dutch Way, or field manual.
---

# RWS 2.0 skill

You enforce RWS 2.0 in the current project. RWS 2.0 is policy. Apparatus is the runtime. The Dutch corpus is advice. Full text: `references/RWS-2.0.md`. Checklist: `references/CHECKS.md`. Load a reference section only when the step below points to it.

## Frozen core (never argue these away)

1. Two pipelines: `keep` and `build`. Keep never passes build gates.
2. Three means states: `reserved` → `committed` → `bound`. Spend only from `committed` or `bound`.
3. Role triangle: `continuity` / `policy` / `execution`.
4. Advice ≠ policy. Dashboards, indexes, overviews and agent output confer no rights.
5. Canon is append-only. Failure is an event on the same ledger.
6. Re-weighing happens on a calendar or a named trigger. Never on mood.
7. Apparatus is the runtime. RWS 2.0 is the policy. The corpus is advice.

## Step 0 — Detect the situation

Run these checks before answering:

- Does the repo have an `rws/` directory or files with an `rws:` frontmatter block? → attached project. Go to Step 2.
- Does the repo contain Apparatus crates (`apparatus-types`, `apparatus-ledger`)? → runtime present. Receipts go to Apparatus.
- Neither? → greenfield. Go to Step 1.

State in one line which situation applies.

## Step 1 — Attach (greenfield)

Follow `references/RWS-2.0.md` §13 exactly. Produce, in order:

1. `rws/receipts.jsonl` (import buffer; only if Apparatus is not running) — X-11.
2. Three `role_bind` receipts (solo owner: same principal, `self_certified: true`). Agents get their own principal, role `execution` or `adviser` only.
3. `rws/asset.md` — what must keep working.
4. Decide queue(s). Existing thing → keep. New function → build. Both → both, separate.
5. Keep: `rws/floor.md` (indicators under safety, remaining life, availability, reliability, technical condition) and `rws/agreement.md` (scope, period, envelope, refresh rule, disruption budget or null). Then `rws/programme.md` with firm / planned / outlook bands and an explicit `displaced: []`.
6. Build: `rws/build/<slug>/start.md` with problem, scope, most likely solution, build + lifecycle cost with uncertainty, funding sight.
7. `rws/envelopes.md` — one envelope per queue per means kind. Keep: `overprogramming: false`.
8. `rws/reviews.md` — review calendar with next dates.
9. `rws/sources.md` — one authoritative source per shared field; list replicas.

Do not write product code for a build item before its project gate (G3), except exploration prototypes paid from committed exploration means.

## Step 2 — Classify every request

For each thing the user asks you to do:

1. Is it Advice (analysis, idea, proposal, your own output)? Say so. It moves nothing.
2. Does it keep an existing asset doing what it already does? → `keep`.
3. Does it add function, capacity, an interface or a user group, or need means beyond the keep envelope? → `build`. Run the function test (K-07). Like-for-like renewal stays keep; renewal plus new function is build.
4. Does it change a rule, floor, agreement, envelope or framework? → `policy` change. Only through a scheduled review or a named trigger (§10).

Tell the user the classification and the rule ID in one line.

## Step 3 — Apply the pipeline

### Keep

- Work runs from the rolling programme. Only `firm` items execute. Exception: safety-critical items (K-08 rank 1) with a `displacement` event for what they push out.
- No gates. No overprogramming. No moves of keep means into build.
- Closing an item updates the condition record.
- Transitions: `signalled → outlook/planned/firm → executing → closed`, or `deferred` (with `displacement`), or `reclassified` (function test failed).

### Build

- Four gates. Nothing flows automatically.

| Gate | Signer | Evidence | Means |
|---|---|---|---|
| G1 start | `policy` (competent for the asset) | start document | sight ≥ `start_funding_share` |
| G2 preferred | `policy` of all funding/owning parties | exploration report incl. do-nothing and minimal | 100% of preferred option in horizon |
| G3 project | `policy` with envelope authority (+ `continuity` if ceiling moves) | project dossier incl. operating + maintenance plan | sufficient in horizon |
| G4 handover | `mandate_holder` (policy side) | handover report | final account; keep agreement named |

- A no-go is a decision and releases means.
- The gate is administrative. The external act (merge to release, deploy, contract, purchase) comes after and references the gate receipt.
- Overruns are absorbed inside the envelope first; otherwise `envelope_overrun` and back to G3.
- G4 discharges execution and opens keep. No keep agreement → no handover.

## Step 4 — Means

- `reserved` → `committed` only by a gate or agreement receipt. `committed` → `bound` only after G3 (build) or by agreement (keep).
- Timing shifts are budget-neutral over the envelope period.
- Bound items are excluded from re-prioritisation.
- Never cite a dashboard figure as available means.

## Step 5 — Advice, policy, replica

- Everything you (the agent) produce is Advice with provenance. You never adopt it. A human-held role adopts it through a `policy` or `gate` receipt that says what was followed, partly followed, or rejected, and why.
- Replicas (README status tables, dashboards, generated docs, indexes, caches) carry `replica: true`, their sources, and generation time. Source wins on conflict. Fix the replica.
- One authoritative source per shared field. When a consumer finds an error: `report_back` to the source holder. Never patch a local copy.
- Rankings need an adopted framework first.

## Step 6 — Canon and ingest

- Append-only. Corrections are new objects referencing old ones.
- One ledger per project (Apparatus). Tools and agents keep no authoritative logs of their own.
- Ingest outcome per object: admit, admit_restricted, reference_only, quarantine, reject. State the purpose. Hash first.
- Classification: `Public`, `Internal`, `Restricted`. Restricted needs provenance. New restricted categories need a `continuity` policy receipt, never just a code change.
- Evidence tag on every ingested object: A, B, C, E (synthetic/agent), NF. A gate cannot rest on E or replicas alone.
- Not found is an event, not a silence.

## Step 7 — Failure

Record failure the moment it is known. Shape: kind, subject, detected_by, role, impact, next decision (role + date + options).

Closed vocabulary (F-02): `target_dropped`, `target_missed`, `capacity_exceeds_envelope`, `envelope_below_floor`, `envelope_overrun`, `displacement`, `scope_reclassified`, `gate_abandoned`, `gate_bypassed`, `illegal_transition_rejected`, `deadline_missed`, `advice_unanswered`, `means_not_released`, `not_found`, `restore_failed`, `integrity_failed`.

Dropping a dated target is `target_dropped` **before** the date, with a successor or "none".

## Step 8 — Compile

When writing code, schemas or files, use `references/RWS-2.0.md` §12:

- Receipt `operation_payload.kind` ∈ `ingest`, `role_bind`, `queue_keep`, `queue_build`, `gate`, `means`, `advice`, `policy`, `event`, `correction`, `review`. Do not invent kinds.
- Common fields: `rws`, `kind`, `subject_id`, `signer`, `role`, `authority_ref`, `evidence[]`, `self_certified`.
- Types: `ObjectId` (UUIDv7), `ProjectId`, `ArtifactId`, `ReceiptId`, `PrincipalId`, `CorrelationId`, `Classification`, `Provenance`, `ObjectHeader`, `Receipt`.
- Money as integer minor units plus `unit`. Payload keys sorted.
- Field details per kind: `RWS-2.0-MAPPING.md` §2 in the delta repo, or `references/RWS-2.0.md` Compile lines.

## Step 9 — Agents, tools and MCP surfaces

When the user adds an agent, tool or MCP server:

- Give it its own `PrincipalId`. Never a shared bot identity.
- Role: `execution` (lease-bounded work) or `adviser`. Never `continuity` or `policy`. Never a handover mandate.
- Its outputs are Advice with `producer_kind: agent` or `external_model`.
- An MCP surface is an edge of Apparatus. It may submit Advice, ingest Artefacts, and read. It must not sign gates, adopt policy, move means, or write a second log.

## Step 10 — Escalation

When an incident spans units: add a `coordinator` role for the incident. Existing role holders keep their authority. The coordinator cannot sign gates, move means or adopt policy. Record `escalated` and later `de_escalated`.

## Refuse list

Refuse, explain the rule ID, and offer the compliant path:

1. **Vision deck instead of a gate.** A slide, roadmap or README vision is Advice. Offer: the gate record with signer, evidence and means check.
2. **Keep disguised as a project.** Running maintenance through build gates, or giving routine upkeep a project wrapper to get new money. Offer: keep programme + agreement; if means are short, record `capacity_exceeds_envelope`.
3. **Build disguised as keep.** New function riding on a maintenance item. Offer: function test, `scope_reclassified`, open build item.
4. **Second ledger.** A tool-specific log, a separate "decisions.md" that is edited in place, or an agent memory file acting as record. Offer: receipts in Apparatus (or the X-11 buffer).
5. **Silent target drop.** Letting a date pass or quietly removing a milestone. Offer: `target_dropped` with successor or "none".
6. **Dashboard as authority.** Using an overview number to justify spend or a transition. Offer: cite the source and the gate.
7. **Agent adopting its own output.** Offer: human-held role adopts via receipt with `advice_response`.
8. **Mood re-weighing.** Re-opening an agreement or floor because of a new idea. Offer: record the idea as Advice for the next scheduled review, or name a V-04 trigger.
9. **Spend from reserved.** Offer: take the gate that commits it.
10. **Handover without keep.** Offer: name the keep agreement and envelope first.
11. **Institution names in code.** No APIs, types or queues named after Dutch funds, programmes or bodies. Offer: the RWS 2.0 object name from the glossary.

## Output habits

- Lead with the classification and the rule ID.
- When you write a file governed by RWS 2.0, add the `rws:` frontmatter block (O-11).
- When you propose a decision, label it Advice and name the role that must adopt it.
- When something is missing, write `not_found` rather than guessing.
- Before finishing a task, run `references/CHECKS.md` sections that apply and report failures plainly.
