---
title: RWS 2.0 — Operating rules for work that must keep existing
version: 1.0.0
status: v1 policy (supersedes v0 field manual; v0 not present in repo, core carried over from session brief)
date: 2026-09-30
runtime: Apparatus (infraax/Aparatus, M0 types)
corpus: infraax/delta Stage 0 unified + G1-1..G1-3 + G2-1..G2-3 brondossiers (advice only)
language: English rules; Dutch source sentences as epigraphs with source ID
---

# RWS 2.0

**What this is.** A policy for any project that must survive its first version: apps, agents, homelabs, hardware, data systems, research corpora. It tells you which objects exist, who may move them, which moves are illegal, and what the runtime must record.

**Three layers. Keep them apart.**

| Layer | What it is | Authority |
|---|---|---|
| Apparatus | Runtime: identity, receipts, artefact store, validation | Records and enforces. Holds no opinions. |
| RWS 2.0 (this file) | Policy: objects, states, gates, forbidden moves | Governs. Changed only by §10 review. |
| Corpus (Dutch dossiers) | Evidence of how one mature machine does it | Advice. Never cited as a reason on its own. |

**How to read a rule.**

- Rule IDs: `P` purpose, `O` objects, `R` roles, `K` keep, `B` build, `M` means, `A` advice/policy/replica, `C` canon, `F` failure, `V` review, `T` telemetry, `X` compile.
- `MUST` / `MUST NOT` are binding. `SHOULD` means: deviate only with a recorded reason.
- `Compile:` states what Apparatus or file frontmatter must contain for the rule to be enforceable. Rules in §4–§9 all carry one.
- Epigraphs quote the Dutch source that the rule translates. Format: `> "…" — <source ID> [tag]`. Tags: `[A]` law/Kamerstuk/budget, `[B]` official programme page, `[V]` verified by direct fetch on 2026-09-30.
- Numbers from the corpus appear only in **Example** boxes. They are illustrations, not defaults you must copy.

---

## §0 Frozen core

These seven statements are frozen. Every other rule in this file elaborates them. If a later rule appears to contradict one, the frozen core wins and the conflict goes to `OPEN.md`.

1. **Two pipelines.** `keep` (continued existence of what already works) and `build` (new or changed function). Keep does not pass build gates.
2. **Three means states.** `reserved` → `committed` → `bound`.
3. **Role triangle.** `continuity` / `policy` / `execution`.
4. **Advice ≠ policy.** A catalogue, dashboard, index, overview or analysis confers no rights.
5. **Canon is append-only.** Failure is an object on the same ledger as success.
6. **Scheduled review ≠ mood of the week.** Re-weighing happens on a calendar or on a named trigger, never on impulse.
7. **Apparatus is the runtime. RWS 2.0 is the policy. The corpus is advice.**

---

## §1 Purpose and non-purpose

### Purpose

- **P-01** Make continued existence the default outcome of work. A thing that ships and then rots is a failure of this policy, not bad luck.
- **P-02** Separate the money, time and attention that keep existing things alive from the means that create new things. Never let one silently eat the other.
- **P-03** Make every decision reproducible: who signed, under which role, on which evidence, at which gate, with which means.
- **P-04** Make failure visible at the moment it is known, as a typed object, not as absence.
- **P-05** Let a stranger pick up any project governed by this file and find: its roles, its agreement, its programme, its open gates, its means, its failures.
- **P-06** Stay portable. The same rules apply to a Rust service, a Raspberry Pi, a research corpus and a multi-agent workflow. Only the Compile targets change.

### Non-purpose

- **P-10** This is not a model of the Dutch state. No object, queue, API or field in any project governed by this file may be named after a Dutch institution, fund, programme or law. Translate the mechanism; drop the name.
- **P-11** This is not a product roadmap. Specific hardware, models, networks and vendors are execution choices made inside a build item.
- **P-12** This is not a second runtime. It does not define storage, networking or a ledger. Apparatus does.
- **P-13** This is not a policy engine specification. v1 enforces rules by schema validation and receipt checks. A dedicated policy evaluator is later work (see §14).
- **P-14** This is not a culture document. It contains no values statements. Every rule names an object, a transition, a refusal and a compile target, or it is not in v1.

---

## §2 Object model

Nine object types. Every object carries an Apparatus `ObjectHeader` (`id`, `created_at`, `project_id`, `classification`, `provenance`, `correlation_id`). Every change to an object is a `Receipt`. Objects are never edited in place; a new version supersedes the old one by reference.

### Overview

| Object | One-line definition | Canonical? | Changed by |
|---|---|---|---|
| `WorkItem` | A unit of work in exactly one queue, with a state | yes | `queue_keep` / `queue_build` / `gate` receipts |
| `Artefact` | Immutable bytes (document, code snapshot, dataset, report) addressed by hash | yes (bytes) | `ingest` receipt; never modified |
| `Advice` | Any proposal, analysis, forecast, signal, dashboard or AI output | yes as record; **no authority** | `advice` receipt |
| `Policy` | A rule, agreement, envelope or framework adopted by a signer holding the right role | yes, authoritative | `policy` receipt |
| `Event` | A fact that happened, including every failure | yes | `event` receipt |
| `Receipt` | Ledger entry proving an operation was accepted; chained by hash | yes; the ledger itself | append only |
| `Role` | A function (`continuity`, `policy`, `execution`, plus auxiliaries) bound to a principal for a scope and period | yes | `role_bind` receipt |
| `Queue` | Named pipeline: `keep` or `build`. Determines which transitions exist | fixed in v1 | not changeable at runtime |
| `Envelope` | A bounded pool of means (money, hours, compute, hardware) with a period, a ceiling and state buckets | yes | `policy` (create/ceiling) and `means` receipts |

### O-01 WorkItem

Fields (minimum):

| Field | Meaning |
|---|---|
| `id` | `ObjectId` (UUIDv7) |
| `queue` | `keep` \| `build` — set once by the queueing receipt; changing it requires a reclassification event (K-07) |
| `state` | Queue-specific state (§4, §5) |
| `asset_ref` | The asset this work keeps alive or creates |
| `lead` | Principal responsible for correct application of this policy to this item (one per item) |
| `envelope_ref` | Envelope the item draws on |
| `adds_function` | `true` if the result does something the asset did not do before |
| `programme_ref` | Optional: parent programme WorkItem (B-12) |

Rules:

- A WorkItem exists only after a `queue_keep` or `queue_build` receipt. Before that, the request is `Advice` (a signal).
- A WorkItem is in exactly one queue at a time.
- A WorkItem has exactly one `lead`.

> "Ieder MIRT-traject heeft een bestuurlijk aangewezen trekker. De trekker is verantwoordelijk voor de correcte toepassing van de spelregels." — G1-2-D01 Spelregels MIRT 2022 §1 [A][V]

### O-02 Artefact

- Bytes are stored content-addressed. The address is the SHA-256 digest. Path form: `sha256/ab/cd/<hex>` (implemented in `apparatus-artifacts::derive_cas_path`).
- An Artefact never changes. A corrected document is a new Artefact linked by a `correction` receipt.
- Every gate names at least one Artefact as its evidence (§5).
- Every Artefact that is derived (summary, index, OCR, AI output, rendered view) records its parent Artefact IDs.

### O-03 Advice

Advice is everything that informs a decision without being one:

- signals and requests ("we should build X", an inspection finding, a bug report);
- analyses, options studies, cost estimates, forecasts;
- AI agent outputs of any kind;
- programme proposals from the execution role;
- dashboards, overviews, catalogues, indexes, search results;
- recommendations from an evaluator or an adviser.

Rules:

- Advice MUST carry provenance: who or what produced it, from which inputs.
- Advice MUST NOT be referenced as the authority of a transition. A transition references a `Policy` or a `gate` receipt; Advice appears only in its `evidence` list.
- Advice may be ignored. Ignoring advice that an evaluator addressed to a role is a recorded act (A-06).

### O-04 Policy

Policy is what binds. Four sub-kinds:

| Sub-kind | Example | Adopted by |
|---|---|---|
| `rule` | "Build items need ≥ X% funding sight at the start gate" | policy role; continuity co-signs if it touches an envelope ceiling |
| `agreement` | Keep agreement for an asset network and a period (K-01) | continuity + policy + execution |
| `envelope` | Creation or ceiling change of an Envelope | continuity (policy role proposes) |
| `framework` | A weighing framework used to rank competing items (V-07) | policy role, after review of objections |

Rules:

- A Policy exists only after a `policy` receipt signed by the required role(s).
- A Policy MUST reference the Advice it was based on and state how that Advice was taken into account (A-03).
- A Policy has a validity period or an explicit "until superseded".

### O-05 Event

- An Event records a fact: something happened, something was found, something failed, a deadline passed.
- Events never express intent. "We plan to" is Advice.
- Failure Events are defined in §9. They are ordinary Events on the same ledger.

### O-06 Receipt

The Apparatus `Receipt` type is the only way state changes:

```text
Receipt {
  header: ObjectHeader,
  previous_receipt_id: Option<ReceiptId>,
  previous_hash: Option<Sha256Digest>,
  operation_payload: JSON   // RWS 2.0 defines its shape: §12
}
```

- One project, one receipt chain.
- A receipt is never edited or deleted. Wrong receipts are corrected by later receipts (C-06).
- Every receipt payload carries `kind`, `role` (the hat the signer wears), `signer` (`PrincipalId`), and `subject_id`.

### O-07 Role

- A Role is a function, not a person. It is bound to a principal by a `role_bind` receipt for a scope (project, asset, envelope) and a period.
- Details in §3.

### O-08 Queue

- Exactly two queues in v1: `keep`, `build`.
- A queue is a state machine definition. It fixes which states exist and which receipts may move an item between them.
- There is no `misc`, `later`, `backlog` or `ops` queue. Unqueued requests are Advice.

### O-09 Envelope

| Field | Meaning |
|---|---|
| `id` | `ObjectId` |
| `means_kind` | `money` \| `hours` \| `compute` \| `hardware` \| other declared unit |
| `period` | Start and end of validity |
| `ceiling` | Maximum total for the period |
| `buckets` | Amounts in `reserved`, `committed`, `bound` (§6) |
| `queue` | `keep` or `build`. One envelope serves one queue. |
| `carry_over` | Rule for unspent amounts at period end (M-08) |
| `overprogramming` | Allowed only for `build` envelopes (M-09) |

- An Envelope is created by a `policy` receipt of sub-kind `envelope`, signed by `continuity`.
- The sum of the three buckets MUST NOT exceed `ceiling`, except by declared overprogramming on a build envelope.

### O-10 Supporting records (not separate types)

These are expressed with the nine types above:

| Record | Expressed as |
|---|---|
| Gate decision | `Receipt` with `kind: gate` + evidence `Artefact` |
| Programme (keep) | `Policy` sub-kind `agreement` + rolling `Artefact` refreshed yearly |
| Condition report | `Artefact` + `event` receipt `condition_reported` |
| Handover / discharge | `Receipt` with `kind: gate`, `gate: handover` |
| Correction | `Receipt` with `kind: correction` referencing the corrected receipt or artefact |
| Replica / overview | derived `Artefact` with `replica: true` (A-08) |

### O-11 Frontmatter form (repos without a running Apparatus)

Every governed markdown or YAML file in a repo MAY carry this block. The block is a declaration; the receipt is the proof (X-11).

```yaml
rws:
  object: work_item | policy | advice | artefact | envelope | role
  id: 01920000-0000-7000-8000-000000000000   # UUIDv7
  queue: keep | build            # work_item only
  state: <queue state>           # work_item only
  lead: <principal>              # work_item only
  role: continuity | policy | execution | adviser | evaluator
  classification: public | internal | restricted
  provenance: { source: "<url|path|system>", creator: "<principal>" }
  evidence: [<artefact id or path>]
  supersedes: <id or null>
```

---

## §3 Roles triangle and naming rule

> "Ten aanzien van het agentschap is er één eindverantwoordelijke binnen het agentschap, één continuïteitsverantwoordelijke en tenminste één beleidsverantwoordelijke." — Regeling agentschappen 2024 art. 6 lid 1 (G1-1-D15) [A][V]

### R-01 The three roles

| Role | Owns | May | Must not |
|---|---|---|---|
| `continuity` | The asset's continued existence and the limits of its means | Create envelopes; approve the agreement, annual plan and annual account; set ceilings; guard the canon | Direct day-to-day activities; sign gates alone |
| `policy` | What the asset must deliver and what new function is wanted | Request products; set performance floors; adopt rules and frameworks; sign start, preferred and project gates; hold the handover mandate | Execute work; move bound means; edit the canon |
| `execution` | Doing the work and accounting for it | Propose programmes and options; run work; manage the envelope's operational spending; account for results; report failures | Adopt policy; raise its own ceiling; sign its own discharge |

> "De beleidsverantwoordelijke en eindverantwoordelijke binnen het agentschap bepalen in onderling overleg de activiteiten van het agentschap…" — Regeling agentschappen 2024 art. 6 lid 2 [A][V]

- **R-02** Per governed unit (project or asset network): exactly one `continuity`, exactly one `execution`, at least one `policy`.
- **R-03** `policy` and `execution` jointly set the activities of a keep agreement. `continuity` approves; it does not co-author.
- **R-04** Execution work agreements cover at least the next three periods (default: three calendar years), refreshed yearly.

> "De werkafspraken hebben in elk geval betrekking op de komende drie kalenderjaren." — Regeling agentschappen 2024 art. 7 lid 2 [A][V]

### R-05 Auxiliary roles (outside the triangle)

| Role | Produces | Cannot |
|---|---|---|
| `lead` | Ensures this policy is applied correctly to one WorkItem | Sign gates in a role it does not hold |
| `adviser` | Advice (proposals, reviews, options). May chair a review (§10). | Adopt policy |
| `evaluator` | Findings on performance, accounts, data quality, incidents | Alter operational records; execute |
| `mandate holder` | Signs a specific gate type on behalf of `policy` (e.g. handover) | Sign outside the mandate's scope |
| `coordinator` | Convenes and sequences units during an escalation (R-10) | Sign gates, move means or adopt policy for any unit |

> "Het Deltaprogramma is daarin adviserend, het Nationaal Water Programma legt het beleid vast." — deltaprogramma.nl/themas/waterveiligheid (G2-3-D14) [B][V]

### R-06 Naming rule

- Name roles by function only: `continuity`, `policy`, `execution`, `lead`, `adviser`, `evaluator`, `mandate_holder`, `coordinator`. Never by title, institution, person or tool ("the SG", "the DG", "Dex", "Claude", "the Pi").
- Bind names to roles through `role_bind` receipts. The binding is data; the policy text never contains a person.
- The triangle replaces the older names `owner` / `principal` / `contractor` (and `eigenaar` / `opdrachtgever` / `opdrachtnemer`). Do not reintroduce them in code or schemas.

> "Voorheen: opdrachtnemer, opdrachtgever en eigenaar" — Toelichting Regeling agentschappen 2024, Stcrt. 2024, 32572 (G1-1-D15) [A]

### R-07 One principal, several roles (solo mode)

A single owner often holds all three roles. This is allowed. Separation then lives in the receipt, not in the person:

- Every receipt names the `role` under which it was signed.
- A principal MUST NOT sign two gates of the same WorkItem under the same role in the same receipt.
- When one principal signs both the evidence and the gate that relies on it, the gate payload carries `self_certified: true`. This is visible, not forbidden.

### R-08 Agents and services

- An AI agent, script or service is a principal with its own `PrincipalId`. It is never the same principal as a human.
- Agents may hold `execution` (for a lease-bounded task) or `adviser`.
- Agents MUST NOT hold `continuity` or `policy`, and MUST NOT hold a handover mandate.
- Agent output is Advice until a human-held role adopts it through a receipt.

### R-09 Role binding lifecycle

| From | To | Receipt | Signer |
|---|---|---|---|
| unbound | bound | `role_bind` `{role, principal, scope, from, until}` | `continuity` (bootstrap: the project creator, self-certified) |
| bound | rebound | new `role_bind` superseding old | `continuity` |
| bound | vacated | `role_bind` with `principal: null` + `event: role_vacated` | `continuity` |

- A vacant `execution` or `continuity` role blocks every gate on that unit until rebound.

### R-10 Escalation adds coordination, never authority

When an incident or problem outgrows one unit (one project, one asset network, one team), escalate by **adding** a coordination role. Do not move authority upward.

- Escalation creates a `coordinator` role bound for the incident's scope and duration. The coordinator convenes, sequences and shares information.
- Every existing role holder keeps its authority over its own unit. The coordinator cannot sign gates, move means or adopt policy for a unit it does not hold roles in.
- Escalating to a higher level does not remove the lower level. Levels run side by side.
- De-escalation is also recorded. An escalation with no de-escalation after the incident closes is an open item at the next review.

> "Burgemeester; GRIP 3 ondersteunt zijn besluitvorming maar creëert op zichzelf geen nieuwe bevoegdheden." — G0-5 (GRIP-tabel) [B]

> GRIP 5: each chair keeps authority in the own region; no transfer upward. The national crisis structure can run beside any regional level and creates no new emergency powers. (G0-5 via STAGE-0-UNIFIED §6) [A/B]

Compile: `event: escalated {incident_ref, level, units: [ProjectId], coordinator: PrincipalId}` plus `role_bind {role: coordinator, scope: {incident_ref}}`; `event: de_escalated`; validator refuses `gate`, `means` and `policy` receipts signed under role `coordinator`.

Compile: `role_bind` payload `{role, principal: PrincipalId, scope: {project_id, asset_ref?, envelope_ref?}, from, until}`; validator rejects any receipt whose `signer` does not hold `role` for the receipt's scope at `created_at`.

---

## §4 Keep pipeline

Keep is the work that makes an existing asset go on doing what it already does: operate, inspect, maintain, repair, renew like-for-like. Keep runs on an **agreement** and a **rolling programme**. It has no start gate, no preferred-option gate, no project gate.

> "Voor overige instandhoudingsopgaven gelden de spelregels niet." — Spelregels MIRT 2022 p. 5 (G1-2-D01) [A][V]

### K-01 The keep agreement

The agreement is a `Policy` of sub-kind `agreement`. It fixes, for one asset network and one period:

1. the **performance floor** the asset must keep meeting (K-03);
2. the **scope**: which assets, which activity types (operate, maintain, renew like-for-like);
3. the **keep envelope** that pays for it (K-04);
4. the three role bindings (R-02);
5. the **refresh rule** for the programme (K-02);
6. the **end date** and what happens at the end (renegotiate, extend, retire).

Rules:

- The agreement is **static** for its period. It is not re-opened because a new idea appeared. It changes only through §10 review.
- The agreement is signed by `continuity`, `policy` and `execution` in one `policy` receipt, or three receipts correlated by `correlation_id`.
- The agreement binds performance and programming *inside* the envelope. It does not authorise spending beyond the envelope. Growth beyond the envelope is a new decision for `continuity`.

> "…enerzijds de meerjarenafspraak die statisch is en loopt van 2024-2030 en anderzijds een Meerjarenprogramma, die 8-jaars voortrollend is…" — Meerjarenplan Instandhouding 2025–2030 §1.2 (G1-1-D10) [A]

Compile: `policy` receipt `{sub_kind: agreement, asset_scope, period, floor_ref, envelope_ref, refresh: {every, horizon}}` with signers covering all three roles; validator refuses a `queue_keep` receipt whose asset is not inside an active agreement's `asset_scope`.

### K-02 The rolling programme

The programme is an `Artefact` that lists the keep work for a horizon. It is proposed by `execution`, agreed by `policy`, approved by `continuity` (R-03).

- The programme has three bands:
  - **firm band** — work that is ready to execute, fully resourced, in sequence;
  - **planned band** — work that is scheduled and estimated but not yet fully prepared;
  - **outlook band** — known future needs, not yet planned.
- The programme is refreshed on a fixed rhythm (default: yearly). Each refresh is a new Artefact superseding the previous one. The old programme stays in the canon.
- Work moves from outlook → planned → firm only at a refresh or through K-08 (urgent safety).

> "…waarvan de eerste 4 jaar maakbaar worden geprogrammeerd en de 4 jaar daarna vooruit worden gepland." — Aanhangsel Handelingen 2025–2026 nr. 682 (G1-1-D21) [A]

**Example (corpus, not a default):** RWS looks 16 years ahead; first 4 years buildable, next 4 planned; programme 8 years rolling, refreshed yearly; agreement fixed 2024–2030.

Compile: programme Artefact with frontmatter `rws.object: artefact`, `programme: {agreement_ref, horizon, bands: {firm, planned, outlook}}`; `policy` receipt `sub_kind: rule` approving each refresh; validator refuses `queue_keep` transition to `executing` for items not in the current firm band (except K-08).

### K-03 Performance floor

- The floor is a `Policy` (`sub_kind: rule`) that states, per asset class, the minimum service the asset must deliver. It is the basis of the agreement.
- The floor must be **realistic, achievable and buildable** with the envelope. A floor that cannot be met with the envelope is recorded as `capacity_exceeds_envelope` or `envelope_below_floor` (F-02) at adoption, not discovered later.
- Every floor names the indicators that prove it (T-02).

> "Het fundament van deze afspraak is het basiskwaliteitsniveau (BKN)." — Meerjarenplan Instandhouding 2025–2030 (G1-1-D10) [A]

Compile: floor Policy `{asset_class, indicators: [{name, target, measured_by}], review}`; `condition_reported` events reference floor indicators by name.

### K-04 Keep envelope

- Keep work draws only from a `keep` envelope.
- The keep envelope's committed amount for the agreement period is treated as `bound` (M-03). It is not available for re-prioritisation into build.
- No overprogramming on a keep envelope. Timing slips inside keep are absorbed inside the keep envelope.

> "Op het onderhoudsprogramma vindt geen overprogrammering plaats, eventuele kasversnellingen en –vertragingen hierop worden opgevangen binnen de begroting van de uitvoeringsorganisaties." — MvT Mobiliteitsfonds 2026 §2.3 (G1-2-D10) [A][V]

Compile: Envelope `{queue: keep, overprogramming: false}`; validator refuses any `means` receipt that moves amounts from a keep envelope to a build envelope, and any keep envelope whose buckets exceed `ceiling`.

### K-05 Keep states

| State | Meaning |
|---|---|
| `signalled` | A need is recorded (inspection finding, failure, wear, obsolescence) — still Advice-backed |
| `outlook` | Accepted into the programme's outlook band |
| `planned` | In the planned band; estimated; sequenced |
| `firm` | In the firm band; resourced; ready |
| `executing` | Work under way |
| `closed` | Work done; condition updated |
| `deferred` | Displaced by higher priority (F-03 `displacement` recorded) |
| `reclassified` | Moved to build (K-07); terminal in keep |

Compile: WorkItem `state ∈ {signalled, outlook, planned, firm, executing, closed, deferred, reclassified}` for `queue: keep`; folded from `queue_keep` receipts (X-09).

### K-06 Keep transitions

| From → To | Receipt | Signer | Required evidence |
|---|---|---|---|
| (Advice) → `signalled` | `queue_keep` | `execution` | signal Artefact or Event |
| `signalled` → `outlook` / `planned` / `firm` | `queue_keep` | `execution`, inside a programme refresh approved per K-02 | programme Artefact |
| `planned` → `firm` | `queue_keep` | `execution`, at refresh | programme Artefact |
| `firm` → `executing` | `queue_keep` | `execution` | none beyond programme |
| `executing` → `closed` | `queue_keep` | `execution` | completion Artefact + `condition_reported` event |
| any → `deferred` | `queue_keep` + `event: displacement` | `execution` | reason; what displaced it |
| any → `reclassified` | `queue_keep` + `event: scope_reclassified` | `execution` or `policy` | K-07 test result |

- Keep items never receive `gate` receipts. If a keep item needs a gate, it is not keep (K-07).
- `closed` needs no discharge gate: the agreement already authorised the work; the annual account (R-01, `continuity`) covers it.

Compile: `queue_keep` payload `{from, to, programme_ref, evidence}`; validator refuses `kind: gate` receipts whose subject is a WorkItem with `queue: keep`.

### K-07 The function test (keep vs build boundary)

Apply this test to every keep item when it enters the programme and again before `executing`:

1. Does the result do anything the asset did not do before (new capability, higher capacity, new interface, new user group)?
2. Does it change the performance floor?
3. Does it need means beyond the keep envelope?

If any answer is yes, the item is build. Record `scope_reclassified`, close it in keep as `reclassified`, and open a build WorkItem at `proposed` (§5) linked by `correlation_id`.

> "…aandacht voor vervanging, modernisering en renovatie van bestaande infrastructuur, waarbij mogelijk vervanging aan de orde is met toevoeging van extra functionaliteit." — Spelregels MIRT 2022 p. 5 (G1-2-D01) [A][V]

- Like-for-like renewal (same function, current standards) stays keep.
- Renewal plus new function is build, even when the renewal part dominates the cost.
- A change of function that "policy" wants during keep work is build. It does not ride along.

Compile: WorkItem field `adds_function: bool`; validator refuses `queue_keep` → `executing` when `adds_function: true`; `event: scope_reclassified {from_item, to_item, test_answers}`.

### K-08 Priority inside keep

When the programme cannot hold everything, rank in this order:

1. Items where safety of people or of the asset's primary protective function is at stake.
2. Items where a statutory or contractual obligation is already bound.
3. Items with the best lifecycle ratio: compare the cost of extending life against renewal, per object.
4. Items that reduce disruption for users.
5. Everything else.

> "…levensduurverlenging als het kan en economisch verstandig is en vernieuwing als dat nodig is. Per object worden de kosten van de levensduurverlengende investering vergeleken met een vernieuwingsinvestering." — Kamerbrief Afweegproces prioritering 19-06-2026 (G2-1-D07) [A][V]

- An item in rank 1 may jump straight to `firm` outside the refresh cycle. The jump is a `queue_keep` receipt with `urgent: safety` and an `event: displacement` for whatever it pushed out.
- Ranking inputs are recorded in the programme Artefact. A reviewer must be able to redo the ranking.

Compile: programme Artefact field `ranking: [{item, rank_class, life_extension_cost, renewal_cost, rationale}]`; `queue_keep` payload `urgent: safety | null`.

### K-09 Displacement is recorded, never silent

When keep capacity or means cannot carry the programme, something is pushed out. That is displacement.

- Every displaced item gets `state: deferred` and `event: displacement {displaced_item, by_item, reason}`.
- The programme refresh lists all displaced items in one table. An empty table is a claim, not a default.
- If displacement is caused by means (capacity to execute is larger than the envelope), also record `capacity_exceeds_envelope` (F-02).

> "De maakbaarheid bij Rijkswaterstaat is inmiddels groter dan het financiële kader (Mobiliteitsfonds en Deltafonds)." — Kamerstuk 36 800 A nr. 9 (G1-1-D12) [A]

Compile: `event` kind `displacement`; programme Artefact field `displaced: [...]` required (may be empty only with `displaced_checked_by` signer).

### K-10 Disruption budget

Keep work disturbs users. Treat disturbance as a spent resource.

- The agreement MAY set a disruption budget: a maximum share of total service disruption that may be caused by keep work in a period.
- Each keep item in `firm` declares its expected disruption class and its window.
- Prefer short full outages in a planned window over long partial degradation, when total disruption is lower.
- Actual disruption is reported per period against the budget.

**Example (corpus, not a default):** since 2005 RWS has held that work-caused congestion on the main road network may not exceed 10% of total congestion in a calendar year. Proposed but unconfirmed additions: ≤60 minutes extra travel time per work, ≥75% user satisfaction. (G2-2-D02 [A])

Compile: agreement field `disruption_budget: {metric, max_share, period} | null`; WorkItem fields `disruption_class`, `window`; `event: disruption_reported {period, actual, budget}`.

### K-11 Condition reporting

- `execution` reports the condition of the asset network on a fixed rhythm (default: yearly), against the floor indicators.
- The report is an Artefact plus an `event: condition_reported`.
- The report states the share of assets approaching end of life. That share drives the outlook band.

**Example (corpus, not a default):** the Staat van de Infrastructuur uses five criteria — safety, remaining life, availability, reliability, technical condition — with a reference date of 1 January. (G0-1, G2-2-D03 [A])

Compile: `event: condition_reported {period, floor_ref, indicators: [{name, value, status}], end_of_life_share}`.

### K-12 Forbidden keep operations

Refuse, and record the refusal as `event: illegal_transition_rejected`:

1. Running a keep item through build gates "to be safe". Gates on keep are noise; they hide the agreement.
2. Hiding new function inside keep (K-07).
3. Moving keep means into build, directly or through a "temporary" loan.
4. Overprogramming a keep envelope.
5. Executing keep work that is not in the firm band, except rank-1 safety (K-08).
6. Re-opening the agreement mid-period because a new idea appeared. Use §10.
7. Deferring an item without a `displacement` event.
8. Closing an item without updating the condition record.

Compile: each refusal is an `event` `{kind: illegal_transition_rejected, rule: "K-12.n", attempted_receipt}`.

---

## §5 Build pipeline

Build is the work that creates new function or changes existing function. It runs through four gates. Nothing flows automatically from one phase to the next.

> "Er is geen automatische doorstroming van een project van de ene naar de volgende fase. Per fase wordt een expliciete bestuurlijke beslissing genomen over het wel of niet (blijven) opnemen (go/no go beslismoment)…" — Spelregels MIRT 2022 §1 (G1-2-D01) [A][V]

### B-01 Phases and gates

```text
proposed ──G1 start──▶ exploring ──G2 preferred──▶ designing ──G3 project──▶ realising ──G4 handover──▶ handed_over
    │                     │                           │                          │
    └──── no-go ──────────┴──────── no-go ────────────┴──────── stop ───────────┴──▶ stopped
```

| Phase | Purpose | Output artefact |
|---|---|---|
| `proposed` | Understand the need; decide whether it is worth exploring | start document |
| `exploring` | Compare real options including do-nothing and minimal; pick one | exploration report |
| `designing` | Make the chosen option buildable, operable, maintainable | project dossier |
| `realising` | Build and deliver | handover report |
| `handed_over` | Asset enters keep (B-10) | — |
| `stopped` | Terminal; reason recorded | stop record |

Compile: WorkItem `state ∈ {proposed, exploring, designing, realising, handed_over, stopped}` for `queue: build`; state is folded from `queue_build` + `gate` receipts (X-09).

### B-02 Gate table

| Gate | Signer (role) | Evidence artefact | Means threshold | Other conditions |
|---|---|---|---|---|
| **G1 start** | `policy` for the unit (the competent authority for the asset) | start document: problem, scope, most likely solution, rough cost incl. lifecycle maintenance cost change | sight on ≥ `start_funding_share` of the most likely solution (M-05) | lead named; function test says build |
| **G2 preferred** | `policy` of every party that funds or owns part of the result, jointly | exploration report: options incl. do-nothing and minimal; lifecycle cost; risks; recommendation | 100% of the preferred option available within the plan horizon | agreement between parties on the preferred option |
| **G3 project** | `policy` holder(s) with authority over the envelope (co-signed by `continuity` when the envelope ceiling moves) | project dossier: design, validation, cost estimate within `design_uncertainty`, risk allocation, operating and maintenance plan, named operator | sufficient means within the plan horizon | cost and risk allocation written down between parties |
| **G4 handover** | `mandate holder` for handover (default: `policy`) | handover report: delivered scope, major deviations, residual risks, lifecycle maintenance cost, efficiency, timeline, lessons | final account | named keep owner and keep envelope (B-10) |

> "Op basis van het startdocument (dat het resultaat is van bestuurlijke besluitvorming) besluit het bevoegd gezag om de verkenningsfase te starten." — Spelregels MIRT 2022 §2.7 [A][V]

> "Er zijn voldoende (100%) financiële middelen beschikbaar om het voorkeursalternatief binnen de voorgestelde planhorizon te realiseren." — Spelregels MIRT 2022 §3.5 [A][V]

> "De projectbeslissing wordt in het BO-MIRT besproken en genomen door betrokken bewindsperso(o)n(en)." — Spelregels MIRT 2022 §4.5 [A][V]

> "De opleveringsbeslissing wordt genomen door de hiervoor gemandateerde Directeur-Generaal, die daarmee ook decharge verleent aan de uitvoeringsorganisatie." — Spelregels MIRT 2022 §5.5 [A][V]

**Example (corpus, not a default):** start threshold 75% (90% for the flood-defence programme); preferred 100%; investment cost uncertainty ±25% at exploration and ±15% at design; maintenance cost uncertainty ±35%. (G1-2-D01 [A])

Compile: `gate` payload `{gate: start|preferred|project|handover, outcome: go|no_go|stop|return, signers: [{principal, role}], evidence: [ArtifactId], means_check: {envelope_ref, required, available, share}, conditions: [...]}`; validator refuses a `gate` receipt whose signers do not hold the table's role, whose evidence list is empty, or whose `means_check.share` is below the project's threshold for that gate.

### B-03 Build states and transitions

| From → To | Receipt | Signer | Evidence |
|---|---|---|---|
| (Advice) → `proposed` | `queue_build` | `policy` or `lead` | signal Artefact |
| `proposed` → `exploring` | `gate` G1 `go` | per B-02 | start document |
| `proposed` → `stopped` | `gate` G1 `no_go` | per B-02 | start document |
| `proposed` → `proposed` (held) | `gate` G1 `hold` | per B-02 | reason + what is needed to proceed |
| `exploring` → `designing` | `gate` G2 `go` | per B-02 | exploration report |
| `exploring` → `stopped` | `gate` G2 `no_go` | per B-02 | exploration report |
| `designing` → `realising` | `gate` G3 `go` | per B-02 | project dossier |
| `designing` → `exploring` | `gate` G3 `return` | per B-02 | reason |
| `realising` → `handed_over` | `gate` G4 `go` | per B-02 | handover report |
| any → `stopped` | `gate` `stop` + `event: stop_recorded` | `policy` | stop record |

- **B-04 A no-go is a decision.** It produces a gate receipt and releases reserved means (M-06). An exploration that fizzles without a G2 receipt is a failure event (`gate_abandoned`, F-02).

> "Er wordt dus ook een voorkeursbeslissing genomen bij een no go." — Spelregels MIRT 2022 §3.5 [A][V]

- **B-05 No gate, no phase.** Work in a phase that the item has not reached is refused. Code, contracts or purchases for `realising` before G3 are an `illegal_transition_rejected` event.
- **B-06 Gates may return.** A gate can send an item back one phase with reasons. Returning is a normal outcome, not a failure.

Compile: `queue_build` payload `{from: null, to: proposed, lead, asset_ref, adds_function: true}`; validator enforces the table; any `means` receipt moving to `bound` for a build item before G3 `go` is refused.

### B-07 The gate is administrative; the act is separate

A gate decides that the organisation will proceed. The binding external act (signing a contract, merging to the release branch, deploying to production, buying hardware) is done afterwards by whoever is competent for that act, and must reference the gate receipt.

> "De bevoegde overheid neemt vervolgens, in lijn met de toepasselijke wettelijk verplichtingen, het besluit in juridische zin. Tegen het besluit in juridische zin staan rechtsmiddelen open." — Spelregels MIRT 2022 §1 [A][V]

- An external act without a gate reference is refused (or, when outside Apparatus's reach, recorded as `gate_bypassed`, F-02).
- A gate receipt without a later act is not a failure; the act may wait. A gate older than its `valid_until` must be re-taken.

Compile: act receipts (e.g. `ingest` of a signed contract, `event: deployed`) carry `gate_ref: ReceiptId`; `gate` payload `valid_until`.

### B-08 Evidence quality per gate

- Every option in the exploration report includes: do nothing; minimal intervention; the preferred intervention; a phased or alternative intervention.
- Every cost figure carries an uncertainty band and a lifecycle part (operate + maintain + renew), not only the build price.
- The start document already includes the change in maintenance cost. A cheap build that raises keep cost is visible at G1.

> "Hierbij worden naast de initiële investeringskosten ook de verandering in de beheer- en onderhoudskosten meegenomen." — Spelregels MIRT 2022 §2.6 [A][V]

Compile: start document and exploration report Artefacts carry frontmatter `estimates: [{option, build, lifecycle_per_year, uncertainty}]`; validator warns (does not refuse) when `lifecycle_per_year` is missing.

### B-09 Overruns stay inside the envelope

- The project envelope set at G3 is a ceiling. An overrun is first absorbed inside that envelope (scope, reserve, sequencing).
- If it cannot be absorbed, record `envelope_overrun` (F-02) and return the item to G3. Do not top up silently from another envelope.

> "Eventuele toekomstige overschrijdingen moeten in beginsel binnen het projectbudget worden ingepast." — Kamerstuk 36 800 A/J nr. 39 (G1-2-D15) [A]

Compile: `event: envelope_overrun {item, envelope, ceiling, forecast}`; validator refuses a `means` receipt that raises a build item's envelope without a new G3 `go`.

### B-10 Handover and discharge

G4 does three things in one receipt:

1. Accepts the delivered scope and records deviations.
2. **Discharges** `execution` for this item: after G4, execution is no longer accountable for delivery; it may still be accountable for operation under keep.
3. Opens the keep side: names the keep agreement (existing or new), the keep envelope, and the first condition baseline.

- When all parties agree, G4 needs no meeting. The mandate holder signs.
- A build item cannot reach `handed_over` if no keep agreement covers the new asset. Build without keep is refused.

> "Indien er overeenstemming is tussen de betrokken partijen, hoeft er in deze fase geen bestuurlijk overleg plaats te vinden." — Spelregels MIRT 2022 §5.5 [A][V]

Compile: `gate` G4 payload `{discharge: {role: execution, principal}, keep: {agreement_ref, envelope_ref, baseline_event_ref}}`; validator refuses G4 `go` without `keep.agreement_ref`.

### B-11 Handover report after use, not only at delivery

The handover report covers realised scope, major deviations, residual risk, lifecycle maintenance cost estimate, efficiency and timeline, and lessons for future builds. It SHOULD be written after an initial period of use (default: within one year of completion), so that it reports reality.

Compile: handover Artefact frontmatter `period_of_use`, `deviations[]`, `residual_risks[]`, `lifecycle_cost`, `lessons[]`.

### B-12 Programmes of small items

Many small build items may run under one programme WorkItem that passes the four gates once for the programme as a whole.

- The programme's G2/G3 fix the envelope and the admission rule for items.
- Items inside the programme are admitted by `execution` against that rule; they do not take their own gates.
- Without recorded programme financing, each component needs its own start-threshold check.

> "…de separate onderdelen ieder 75% zicht op financiering hebben, tenzij er programmafinanciering is vastgelegd." — Spelregels MIRT 2022 §2.6 (paraphrase in G1-2 §3.2) [A]

Compile: WorkItem `programme_ref`; programme Policy `admission_rule`; validator skips gate checks for items with `programme_ref` whose programme is in `realising` and refuses items that exceed `admission_rule`.

### B-13 Reserve capacity check (at G3)

When a build opens up an asset or an interface, check whether a small preparation now prevents a large cost later (a spare interface, a versioned field, an extra conduit, a documented extension point).

- Record the decision as one of: build now / record and defer / reject as speculative.
- Reserve capacity must be justified by a foreseeable need; it is not permission to overbuild.

Source status: this rule comes from the 1.0 Field Manual. The A/B corpus did not confirm a reserve-capacity rule (G2-2 [NF]). Kept because it is an object (a decision record), not a slogan. See `OPEN.md`.

Compile: project dossier field `reserve_capacity: [{item, decision: build|defer|reject, reason}]`.

### B-14 Forbidden build operations

1. Starting work of a later phase before its gate (B-05).
2. Treating momentum ("we already started") as a gate.
3. A gate with no evidence Artefact, or with only Advice from the party that will execute (R-07 `self_certified` must then be set).
4. Topping up an envelope without re-taking G3 (B-09).
5. Handing over without a keep agreement (B-10).
6. Letting an exploration die without a G2 receipt (B-04).
7. Signing G4 as the executor being discharged.
8. Using the build pipeline to run routine maintenance (K-12.1 mirror).

Compile: each refusal is `event {kind: illegal_transition_rejected, rule: "B-14.n", attempted_receipt}`.

---

## §6 Means states and movement rules

Means are anything with a ceiling: money, hours, compute credits, hardware, operator attention. Every unit of means sits in one envelope and in one of three states.

> "Als juridisch verplichte uitgaven worden beschouwd: realisatieprojecten en programma's, DBFM-contracten, apparaatsuitgaven en Exploitatie, Onderhoud en Vernieuwing (Instandhouding). De projecten en programma's die in de planuitwerkings- en verkenningsfase zitten, worden gezien als bestuurlijk gebonden. (Risico)reserveringen zijn beleidsmatig gereserveerd." — MvT Mobiliteitsfonds 2026, budgetflexibiliteit (G1-2-D10) [A][V]

### M-01 The three states

| State | Meaning | Who may move it out | Spend allowed? |
|---|---|---|---|
| `reserved` | Earmarked by policy for a purpose; no item holds it yet, or an item holds it provisionally | `policy` (re-earmark), `continuity` (release) | **no** |
| `committed` | Assigned to a specific WorkItem by a gate or agreement; administratively promised | `policy` via a gate `no_go`/`stop`/`return`; otherwise stays | yes, for the item's current phase |
| `bound` | Contracted, legally or technically irreversible (signed contract, purchased hardware, running keep agreement) | nobody; it can only be spent or written off by `event` | yes |

- **M-02** Spending is allowed only from `committed` or `bound`. A spend request against `reserved` is refused.
- **M-03** Keep envelopes are `bound` for the agreement period (K-04).
- **M-04** `committed` is still re-decidable by `policy` until it becomes `bound`. Record every such change; do not treat `committed` as sacred.

> "Overigens geldt ook dat waar wél bestuurlijke afspraken zijn gemaakt, maar er nog geen juridische verplichtingen zijn aangegaan, de budgetten nog altijd onverminderd door de Tweede Kamer te amenderen zijn." — MvT Mobiliteitsfonds 2026 §2.4 [A][V]

Compile: Envelope buckets `{reserved, committed, bound}`; `means` payload `{envelope_ref, item_ref?, from_state, to_state, amount, unit, gate_ref?}`; validator refuses spend events (`event: spent`) against any item whose `committed + bound` for that envelope is less than the amount.

### M-05 Movement rules by gate

| Moment | Movement | Receipt |
|---|---|---|
| Envelope created | ceiling set; everything starts in `reserved` or unallocated | `policy` `sub_kind: envelope` |
| G1 start `go` | exploration cost: `reserved` → `committed`. Full-option estimate stays `reserved` (earmarked to the item) | `means` with `gate_ref` |
| G2 preferred `go` | full-option means: `reserved` → `committed`, transferred to the item's own line | `means` with `gate_ref` |
| G3 project `go` | means stays `committed`; contracts signed after G3 move amounts to `bound` | `means` with `gate_ref`, then `means` with `act_ref` |
| Keep agreement adopted | keep envelope amounts → `bound` for the period | `means` with `agreement_ref` |
| No-go / stop | item's `reserved` and unspent `committed` → back to pool (`reserved`, unallocated) | `means` with `gate_ref` |

> "Na besluitvorming, zoals een voorkeursbeslissing, wordt budget overgeheveld naar het desbetreffende productartikel." — MvT Mobiliteitsfonds 2026, art. 11 (G1-2-D10) [A][V]

> "Als er wordt besloten om na deze fase definitief te stoppen … vallen de gereserveerde middelen vrij." — Spelregels MIRT 2022 §3.5 [A][V]

Compile: validator refuses `reserved → committed` without `gate_ref` or `agreement_ref`; refuses `committed → bound` for build items before G3 `go`; requires a `means` release receipt after every `no_go`/`stop` gate (missing release within one review cycle → `event: means_not_released`).

### M-06 Start threshold (funding sight)

- The start gate requires sight on a declared share of the most likely solution's cost, including the lifecycle maintenance change. The share is a project policy parameter `start_funding_share`.
- "Sight" means: the means exist in some envelope as `reserved` or are credibly promised by a named party. It does not mean `committed`.
- When there is no single most likely solution, use the average of the credible options.

**Example (corpus, not a default):** 75% at start of exploration; 90% for the flood-defence programme; internal guideline reportedly 100% [C]. The 75% rule was due for review in 2023 and remained unchanged through September 2026. (G1-2 §3.2 [A])

Compile: project Policy `{start_funding_share: 0.0–1.0}`; G1 `means_check.share ≥ start_funding_share`.

### M-07 Cash timing moves are budget-neutral

- Moving means between years (earlier or later) inside one envelope is allowed. The multi-year total does not change.
- A swap between two lines that is reversed in a later year is allowed if it nets to zero over the envelope period.
- Neither move changes a means state.

> "Kasschuiven zijn altijd budgetneutraal, hetgeen betekent dat de hoeveelheid middelen die meerjarig beschikbaar is niet wijzigt als gevolg van de kasschuif." — MvT Mobiliteitsfonds 2026 [A][V]

Compile: `means` payload `{timing_shift: {from_period, to_period, amount}}`; validator checks the envelope's multi-period sum is unchanged.

### M-08 Carry-over at period end

- Each envelope declares its carry-over rule. Default for build envelopes: unspent means carry over in full. Default for keep envelopes: carry-over inside the keep envelope only.
- Unspent build means never fall into a keep envelope, and vice versa, without a `policy` receipt signed by `continuity`.

**Example (corpus, not a default):** the Mobility Fund has a 100% year-end carry-over margin. (G1-2-D10 [A][V])

Compile: Envelope `carry_over: full | none | within_queue | <share>`.

### M-09 Overprogramming (build only)

- A build envelope MAY plan more work than its ceiling, because build items slip. The overshoot is declared per year.
- The ceiling still binds actual spending. Overprogramming is a planning tool, not extra money.
- Keep envelopes never overprogram (K-04).

> "Het instrument overprogrammering wordt als instrument ingezet om te voorkomen dat programmavertragingen direct tot een voordelig saldo leiden…" — MvT Mobiliteitsfonds 2026 §2.3 [A][V]

Compile: Envelope `overprogramming: {allowed: bool, planned_excess_by_period}`; validator refuses `allowed: true` on `queue: keep`; refuses spend beyond `ceiling`.

### M-10 Bound means are outside re-prioritisation

When a portfolio is re-weighed (V-07), items whose means are already `bound` are excluded from the weighing. Everything else is on the table, including keep items not yet bound and committed build items.

> "Er worden vooraf geen projecten of programma's uitgesloten van de prioritering, met uitzondering van de (deel)projecten waarbij reeds juridische verplichtingen zijn aangegaan." — Kamerbrief Afweegproces prioritering 19-06-2026 (G2-1-D07) [A][V]

Compile: re-prioritisation Artefact lists `excluded_bound: [item_ref]`; validator refuses a `means` receipt `bound → reserved`.

### M-11 Forbidden means operations

1. Spending from `reserved`.
2. Moving keep means into build, or build into keep, without `continuity`.
3. Treating a dashboard figure as available means (A-08).
4. Raising a ceiling by spending past it.
5. Leaving no-go means stranded on a dead item.
6. Reclassifying `bound` as `reserved`.

Compile: each refusal → `event {kind: illegal_transition_rejected, rule: "M-11.n"}`.

---

## §7 Advice, policy and replica

Three kinds of text look alike and must never be confused: advice (what someone recommends), policy (what binds), and replica (a copy or view of something else).

### A-01 Advice never binds

- An analysis, a model output, a proposal, an overview, a recommendation: all Advice. None moves a WorkItem, a means state or a role.
- Advice from a respected adviser is still Advice. Seniority does not convert it.

> "Het Deltaprogramma is daarin adviserend, het Nationaal Water Programma legt het beleid vast." — deltaprogramma.nl/themas/waterveiligheid [B][V]

Compile: validator refuses any receipt whose `authority_ref` points to an object of type Advice.

### A-02 Adoption is a receipt

- Advice becomes binding only when the holder of the right role adopts it in a `policy` receipt (O-04) or uses it as evidence in a `gate` receipt.
- The adopting receipt references the Advice by ID. The Advice itself does not change.

> "In het Deltaprogramma 2027 is de kabinetsreactie op het advies van de deltacommissaris opgenomen." — Aanbieding MIRT Overzicht 2027 en DP2027, 15-09-2026 (G1-3-D02) [A][V]

Compile: `policy` payload `{sub_kind, adopts: [AdviceId], signers}`.

### A-03 Say what you did with the advice

Every `policy` receipt that adopts, amends or rejects Advice states, per piece of Advice, whether it was followed, followed in part, or not followed, and why.

> Waterwet art. 4.9 lid 7: the programme states how the commissioner's proposal and advice were taken into account. (paraphrase, G1-3 §3) [A]

Compile: `policy` payload `advice_response: [{advice_id, response: followed|partial|rejected, reason}]`; validator refuses a `policy` receipt with `adopts` non-empty and `advice_response` missing.

### A-04 Signals are Advice until queued

A request, idea, bug report or inspection finding is Advice. It enters a pipeline only through `queue_keep` or `queue_build` (O-01). Nobody starts work on a signal directly.

Compile: validator refuses `event: work_started` whose subject is not a WorkItem.

### A-05 Agent output is Advice

- Every output of an AI agent, script or external model is Advice with provenance `{source: model/tool id + version, creator: agent PrincipalId}`.
- It may be adopted (A-02). It may never adopt itself.

Compile: `advice` payload `{producer: PrincipalId, producer_kind: human|agent|service|external_model, inputs: [ids]}`; validator refuses `policy` or `gate` receipts signed by a principal with `producer_kind != human`.

### A-06 Evaluator findings require a response

- Findings addressed by an `evaluator` to a role are Advice, but the addressed role MUST answer them in a recorded `policy` receipt within the response window of the project policy.
- No answer by the deadline → `event: advice_unanswered` (F-02).

> OvV: addressed ministers must respond to recommendations. (G0-5, Rijkswet OvV) [A]

Compile: `advice` payload `{addressed_to: role, respond_by}`; scheduler emits `advice_unanswered` after `respond_by`.

### A-07 One authoritative source per field

- Every data field that other work relies on has exactly one authoritative source (a registry, a table, a file). The source is declared.
- Consumers read from the source. When they find an error, they report it back to the source holder (`event: report_back`) instead of fixing their own copy.
- The source holder resolves report-backs on a declared rhythm. Unresolved report-backs are visible.

> Authentic data is designated per registration law; use is mandatory where the law says so; report-back and correction loops apply. (G0-4, G0-6) [A/B]

Compile: registry Artefact `authoritative_sources: [{field, source_ref, holder_role}]`; `event: report_back {field, value_seen, suspected_value, reporter}`; `event: report_back_resolved`.

### A-08 A replica confers no rights

An overview, catalogue, dashboard, search index, cache, rendered report, status page or summary is a **replica**. It is a derived Artefact.

- Every replica carries: its source refs, its generation time, and the flag `replica: true`.
- When replica and source disagree, the source wins. Fix the replica, not the source.
- No transition, spend or role may cite a replica as authority.
- A replica published to others SHOULD carry the disclaimer line: "No rights can be derived from this overview; source of truth: <ref>."

> "Aan dit overzicht of de inhoud daarvan kunnen geen rechten worden ontleend." — MIRT Overzicht 2027, disclaimer (G1-2-D09) [A][V]

> "Het MIRT Overzicht is een naslagwerk…" — Spelregels MIRT 2022 [A][V]

Compile: derived Artefact frontmatter `replica: true`, `sources: [ids]`, `generated_at`; validator refuses `authority_ref` or `evidence` entries on gate receipts that point only to replicas.

### A-09 Frameworks before rankings

When competing items must be ranked, first adopt the weighing framework as a Policy (`sub_kind: framework`). Then produce the ranking as Advice that applies the framework, traceably. Then adopt the ranking.

> "…stelt het kabinet het afweegkader vast. Op basis van het afweegkader wordt vervolgens een transparante en herleidbare prioritering opgesteld." — Kamerbrief Afweegproces prioritering 19-06-2026 [A][V]

Compile: ranking Advice payload `{framework_ref, inputs, scores, method}`; validator refuses adoption of a ranking whose `framework_ref` is not an adopted Policy.

### A-10 Forbidden

1. Citing a dashboard as the reason for a spend.
2. An agent adopting its own output.
3. Patching a local copy of an authoritative field.
4. A ranking with no adopted framework behind it.
5. A policy receipt that is silent about the advice it was based on.

Compile: each refusal → `event {kind: illegal_transition_rejected, rule: "A-10.n"}`.

---

## §8 Canon: ingest, classification, provenance

The canon is the set of Artefacts and Receipts of a project. It is append-only.

### C-01 Append-only

- Nothing in the canon is edited or deleted in place.
- Change = new object + receipt that references the old one.
- Derived views (indexes, embeddings, summaries, dashboards) are not canon. They can be deleted and rebuilt from canon at any time.

Compile: `apparatus-store::ArtifactStore::store` MUST fail when the id exists; `ReceiptLedger::append` is the only write path for receipts.

### C-02 One ledger per project

- A project has one receipt chain in Apparatus. Tools, agents and services do not keep their own authoritative logs.
- Tool logs are Advice or telemetry (§11). They may be ingested as Artefacts.

Compile: `ProjectId` on every `ObjectHeader`; one chain head per `ProjectId`.

### C-03 Ingest

Every object entering the canon passes one ingest decision:

| Outcome | Effect |
|---|---|
| `admit` | Store bytes as Artefact; write `ingest` receipt |
| `admit_restricted` | As `admit` with `classification: Restricted` and mandatory provenance |
| `reference_only` | Store metadata and external locator, not bytes |
| `quarantine` | Store separately until provenance/rights/safety is settled; not usable as evidence |
| `reject` | Do not store content; write a minimal `ingest` receipt with `outcome: reject` and reason |

- Purpose before ingest: the ingest receipt states why the object is kept.
- Hash first: the Artefact id is bound to its SHA-256 digest before any extraction.

Compile: `ingest` payload `{outcome, purpose, artifact_id?, digest, size_bytes, media_type, source_locator, reason?}`.

### C-04 Classification

Three classes, matching Apparatus `Classification`:

| Class | Meaning | Rule |
|---|---|---|
| `Public` | May leave the project unchanged | Provenance optional but recommended |
| `Internal` | Project-internal operations | Provenance recommended |
| `Restricted` | Needs explicit authorisation to read (personal, secret, legal, security) | Provenance mandatory (enforced by `apparatus-schema`) |

- New categories of Restricted data (a data type the project never held before) require a `policy` receipt by `continuity`. Code changes do not create new sensitive scope.

Compile: `ObjectHeader.classification`; `Validate for ObjectHeader` rejects `Restricted` without provenance (implemented); `policy` `sub_kind: rule` `{restricted_category_added: name}` required before first ingest of that category.

### C-05 Provenance

- `Provenance { source, creator }`: `source` is the URL, path or system; `creator` is the `PrincipalId` that produced or supplied it.
- Evidence strength is recorded with the corpus tags: `A` (law, contract, own measurement, budget), `B` (official programme text, audited report), `C` (secondary, press, pre-publication), `NF` (not found), plus `E` for synthetic/agent-derived. Store it in the ingest payload.
- An `E` object cannot on its own be the evidence of a gate. It must lead back to at least one `A` or `B` object.

Compile: `ingest` payload `evidence_tag: A|B|C|E|NF`; validator refuses `gate` receipts whose evidence set contains only `E` or `C` objects (warn only for `C`).

### C-06 Corrections

- A wrong receipt or Artefact is corrected by a `correction` receipt `{corrects: id, reason, replacement: id?}`.
- Consumers resolve the latest non-superseded version by following correction links.
- Lawful removal (privacy, rights) replaces bytes with a tombstone Artefact and keeps the receipt with `outcome: removed`. The chain stays intact.

Compile: `correction` payload `{corrects, reason, replacement?, removal: bool}`.

### C-07 Time

- `created_at` is Unix milliseconds from a `Clock` (`apparatus-time`). Tests use `DeterministicClock`.
- Imported objects keep their original occurrence time in the payload (`occurred_at`). They never pretend to have happened at ingest time.

Compile: payload `occurred_at` for ingested historical objects; `ObjectHeader.created_at` is ingest time.

### C-08 Not found is an object

When a required source, document or fact is looked for and not found, write it down as `event: not_found {what, where_searched, when}`. Absence is data. It stays in the canon until superseded by a finding.

> The corpus index keeps "NIET GEVONDEN" rows instead of deleting them. (INDEX-WORK-FROM-TRACKS) [B]

Compile: `event` kind `not_found`.

---

## §9 Failure events

Failure is an Event on the same ledger as everything else. It has a kind, a subject, a time, a detecting principal, an impact statement and a next decision.

> "De ambitie om eind 2025 certificeerbaar te zijn is daarmee niet meer haalbaar." — Kamerstuk 29 385 nr. 142 (G1-1-D08) [A]

### F-01 Failure event shape

```json
{
  "kind": "event",
  "event": "<failure kind>",
  "subject_id": "<WorkItem | Policy | Envelope | Artefact id>",
  "detected_by": "<PrincipalId>",
  "role": "<role of detector>",
  "detected_at": 0,
  "impact": "<what is now worse or at risk>",
  "next_decision": { "role": "<who decides>", "by": 0, "options": ["..."] },
  "refs": ["..."]
}
```

- `impact` and `next_decision` are mandatory. A failure without a next decision is only half recorded.

Compile: `event` payload with the fields above; validator refuses F-02 kinds without `impact` or `next_decision.role`/`next_decision.by`.

### F-02 Failure kinds (v1)

| Kind | Trigger | Next decision owner |
|---|---|---|
| `target_dropped` | A dated target is abandoned or declared unreachable | `policy`, must set a successor target or state "none" |
| `target_missed` | A dated target passes unmet without a prior `target_dropped` | `policy` |
| `capacity_exceeds_envelope` | Execution can do more than the envelope pays for | `continuity` |
| `envelope_below_floor` | The envelope cannot pay for the performance floor | `continuity` + `policy` |
| `envelope_overrun` | Forecast cost exceeds the item's envelope | `policy` (re-take G3) |
| `displacement` | Keep item pushed out by another | `execution` records; `policy` reviews at refresh |
| `scope_reclassified` | Keep item failed the function test | `policy` (opens build) |
| `gate_abandoned` | Build item stalled without a gate receipt past its review date | `policy` |
| `gate_bypassed` | External act without a gate reference | `policy` + `continuity` |
| `illegal_transition_rejected` | Validator refused a receipt | `lead` of the item |
| `deadline_missed` | A statutory or policy-mandated deliverable (evaluation, audit, report) passed its due date | the role that owed it |
| `advice_unanswered` | Addressed finding unanswered by `respond_by` | addressed role |
| `means_not_released` | No-go/stop without means release | `policy` |
| `not_found` | Required source not found (C-08) | `lead` |
| `restore_failed` | Backup restore test failed | `execution` + `continuity` |
| `integrity_failed` | Receipt chain or artefact hash check failed | `continuity` |

**Example (corpus, not a default):** `target_dropped` — ISO 55001 certification by end 2025 declared unreachable in April 2025; the successor date was announced but not found (G1-1). `deadline_missed` — the Wet Mobiliteitsfonds art. 9 evaluation was due by 1 July 2026 and was not found; the budget annex says "te starten" (G1-2). `deadline_missed` — BRK external audit ran one year past its three-year cycle (G0-4).

Compile: `event` payload per F-01; validator refuses `event` receipts whose `event` value is neither in this table nor in the non-failure registry (X-12), unless the project policy registers an additional kind.

### F-03 No silent target drop

- A target with a date is a Policy. Dropping it is a `target_dropped` Event **before** the date passes, with a successor or an explicit "none".
- A date that passes without either a success event or a `target_dropped` is recorded as `target_missed` by the scheduler. Both are failures; the second is worse because it was silent.

Compile: Policy `targets: [{name, due, measure}]`; scheduler emits `target_missed` when `due < now` and no `target_met`/`target_dropped` references the target.

### F-04 Failure is not blame

- A failure event names the role that decides next, not the person who is at fault.
- Correcting a failure is a normal receipt. Deleting it is impossible (C-01).

Compile: `next_decision.role` holds a role name, never a `PrincipalId`; failure events are closed by a later receipt with `refs` to the failure, never by `correction {removal: true}`.

### F-05 Failure feeds review

- Every open failure event with `next_decision.by` in the past is listed at the next scheduled review (§10).
- A cluster of the same kind (default: three in one review period) is a named trigger for an out-of-cycle review (V-04).

Compile: review Artefact section `open_failures[]` generated from the ledger; trigger rule `failure_cluster: {kind, count, period}` in project policy.

---

## §10 Review cadence

Rules, agreements, floors and frameworks change on a schedule or on a named trigger. Never because of the mood of the week.

### V-01 The review calendar

Every project declares these rhythms in its policy. Defaults in brackets.

| Review | What it re-decides | Rhythm | Output |
|---|---|---|---|
| Programme refresh | Keep programme bands, ranking, displacement list | [yearly] | new programme Artefact + `policy` approval |
| Condition report | Asset state vs floor | [yearly] | `condition_reported` event |
| Account | Means spent vs envelope, per state | [yearly] | account Artefact + `continuity` approval |
| Replica publication | Overviews and dashboards regenerated from canon | [yearly, or on every refresh] | replica Artefacts |
| Source audit | Accuracy of each authoritative source (A-07) | [every 3 years] | evaluator findings |
| Policy evaluation | Does a rule do what it was adopted for? | [within 5 years of adoption] | evaluator findings + response |
| Quality policy review | This file's application in the project | [every 2 years] | review Artefact |
| Strategic re-weighing | Floors, agreements, framework, long-term direction | [every 6 years] | re-weighing Advice + adoption |

> "Onze Minister zendt binnen vijf jaar na de inwerkingtreding van deze wet aan de Staten-Generaal een verslag over de doeltreffendheid en de effecten van deze wet in de praktijk." — Wet Mobiliteitsfonds art. 9 (G1-2-D05) [A]

**Example (corpus, not a default):** Delta programme: yearly programme, six-yearly re-weighing (2021, 2027); flood-defence assessment at least every 12 years; BRK external audit at least every 3 years; CBS quality policy reviewed at least every 2 years; law evaluations within 5 years. (G0-3, G0-4, G0-6, G1-2)

Compile: project Policy `review_calendar: [{review, rhythm, next_due, owner_role}]`; scheduler emits `deadline_missed` when `next_due` passes without the review's output receipt.

### V-02 Strategic re-weighing is a procedure

A strategic re-weighing runs through fixed steps. Each step produces an Artefact.

1. **Start note** — scope, questions, what is fixed, who takes part.
2. **Analysis** — what changed since the last re-weighing (condition, demand, failures, means).
3. **Options** — including "no change".
4. **Assessment** — options against the floor, the envelope and the failure record.
5. **Weighing and proposal** — recommended changes, as Advice.
6. **Adoption** — `policy` receipt by the right roles, with A-03 responses.

> Six steps: startnotitie → analyse → opties → beoordeling → afweging/voorstel → advisering/besluitvoorbereiding. (G0-3 via STAGE-0-UNIFIED §4) [B]

Compile: re-weighing WorkItem is not a keep or build item; it is a `review` receipt chain `{review: strategic, step: 1..6, artefact_ref}`; adoption is a `policy` receipt referencing step 5.

### V-03 "No change" is a valid, recorded outcome

A re-weighing that finds no reason to change is a success, not wasted effort. Record it as an adopted Policy that re-affirms the existing one.

> "In het Deltaprogramma 2027 is de Deltabeslissing herijkt. Hieruit bleek dat er op dat moment geen aanleiding was om de doelstellingen of de systematiek te wijzigen." — deltaprogramma.nl/themas/waterveiligheid [B][V]

Compile: `policy` payload `{sub_kind, reaffirms: PolicyId}`.

### V-04 Named triggers for out-of-cycle review

Outside the calendar, a review may start only on a trigger named in the project policy. Allowed trigger types:

- a failure cluster (F-05);
- `envelope_below_floor` or `capacity_exceeds_envelope` recorded;
- a change in an external obligation (law, contract, licence, platform terms);
- an evaluator finding that the addressed role accepts as review-worthy;
- loss of a role holder (R-09).

"New idea", "someone asked", "it feels wrong" are not triggers. They are signals (Advice) for the next scheduled review.

Compile: `review` payload `{trigger: {type, ref}}`; validator refuses `review` receipts with `rhythm: out_of_cycle` and no `trigger.ref` to an existing Event or Artefact.

### V-05 Agreements are not re-opened mid-period

A keep agreement changes during its period only through V-04 with a trigger of type `envelope_below_floor`, `capacity_exceeds_envelope` or external obligation. Otherwise it waits for its end date or the strategic re-weighing.

Compile: validator refuses a `policy` receipt superseding an active `agreement` without `trigger.ref` of the allowed types.

### V-06 Horizon discipline

- Keep plans look further ahead than build plans. The keep outlook band covers at least two refresh cycles beyond the firm band.
- Regular maintenance is planned for a minimum horizon (default: eight years) so execution and suppliers can prepare.

> "Voor wat betreft het reguliere beheer en onderhoud van het areaal, is dat voor een periode van minimaal acht jaar." — Kamerbrief Afweegproces prioritering 19-06-2026 [A][V]

Compile: programme Artefact `horizon ≥ project.min_keep_horizon`.

### V-07 Portfolio re-prioritisation

When means cannot carry both pipelines, re-prioritise across keep and build together, in this order:

1. Adopt the weighing framework (A-09).
2. Exclude `bound` items (M-10).
3. First fund items where safety is at stake.
4. Then apply the framework to the rest, including life-extension vs renewal per asset (K-08).
5. Adopt the ranking; apply means movements in the next account period.

> "Het gaat om exploitatie, onderhoud, vernieuwing én nieuwe aanleg. We bekijken dit niet per project, regio of fonds, maar over de gehele breedte van de opgave…" — Kamerbrief Afweegproces prioritering 19-06-2026 [A]

- Re-prioritisation does not merge the queues. Items keep their queue; only means allocations change.

Compile: re-prioritisation Artefact `{framework_ref, excluded_bound[], safety_first[], ranking[]}`; resulting `means` receipts reference it.

---

## §11 Measurement and telemetry

Measure what steers a decision. Everything else is noise with a storage cost.

### T-01 Telemetry is evidence, not canon truth

- Raw measurements from the system itself (latencies, error counts, availability, disk health) may be ingested as `A`-tagged Artefacts.
- Aggregates, scores and dashboards are replicas (A-08).
- A measurement never changes a state by itself. It triggers an Event, and a role decides.

Compile: telemetry batches ingested with `evidence_tag: A`, `provenance.source: system:<name>`; aggregates flagged `replica: true`.

### T-02 Default condition criteria

Every asset floor names indicators under at least these five headings, or states why one does not apply:

| Criterion | Question |
|---|---|
| safety | Can it hurt people, data or other assets? |
| remaining life | How long until renewal is due? |
| availability | Is it there when needed? |
| reliability | Does it do the right thing when there? |
| technical condition | What state are its parts in? |

> Staat van de Infrastructuur uses five criteria: veiligheid, levensduur, beschikbaarheid, betrouwbaarheid, technische conditie. (G0-1, G2-2-D02) [A]

Compile: floor Policy `indicators[].criterion ∈ {safety, remaining_life, availability, reliability, technical_condition}`.

### T-03 An indicator that is not used to steer is flagged

- Indicators exist to change decisions. An indicator that is measured but not yet used in any policy, gate or programme is flagged `steering: false`.
- Every review lists indicators with `steering: false` and decides: start steering on it, or stop measuring it.

> Rekenkamer 2024: five indicators developed, not yet used that year for formal steering of RWS. (G0-1 via STAGE-0-UNIFIED §2) [A]

Compile: floor Policy `indicators[].steering: bool`; review Artefact section `unsteered_indicators[]`.

### T-04 Complete constraint register

- Keep a complete register of known constraints and restrictions on the asset (known defects, reduced capacity, workarounds, temporary limits).
- An incomplete register is a failure (`not_found` for the missing class of constraint), not an acceptable state.

> Rekenkamer 2023: the minister had no complete overview of restrictions on the main road network. (G0-1) [A]

Compile: registry Artefact `constraints: [{asset, restriction, since, until?, ref}]`; review checks it covers every asset in the agreement scope.

### T-05 Measurement independence

- Whoever measures or evaluates decides the method and the publication moment. The roles that are measured set the budget for measurement, not its content or timing.
- An evaluator's output is published as produced; corrections follow the correction protocol (C-06), openly.

> Wet op het CBS art. 18: the director-general is independent in methods and in the decision to publish statistical results; the minister sets budget and frame, not content or timing. (G0-6) [A]

Compile: `advice` payload from `evaluator` role `{method_ref, published_at}`; validator refuses a `correction` of evaluator output signed by a non-evaluator role.

### T-06 Disruption is measured

Keep work that disturbs users is measured against the disruption budget (K-10). Unmeasured disruption counts as over budget at the next review.

Compile: `event: disruption_reported` per period; missing report → `deadline_missed`.

### T-07 Restore tests are measurements

A backup that has never been restored is a claim. Schedule restore tests; record each as an Event (`restore_tested` or `restore_failed`).

Source status: 1.0 (Field Manual, RWS-Blueprint, The Apparatus). The A/B corpus did not address backup; kept because it is an Event with a clear trigger.

Compile: `event: restore_tested {artefact_set, target, duration, result}`; scheduler rhythm in `review_calendar`.

---

## §12 Compile rules for Apparatus

This section is the contract between RWS 2.0 and the runtime. Everything above that says `Compile:` resolves to what is here.

### X-01 Types used (present in `infraax/Aparatus` M0)

| Type | Crate | RWS 2.0 use |
|---|---|---|
| `ObjectId` (UUIDv7) | apparatus-types | id of every object |
| `ProjectId` | apparatus-types | one governed project = one chain |
| `ArtifactId` | apparatus-types | Artefacts, programmes, reports, replicas |
| `ReceiptId` | apparatus-types | every state change |
| `PrincipalId` | apparatus-types | humans, agents, services; signers |
| `CorrelationId` | apparatus-types | links multi-receipt decisions (three-signer agreement; reclassification pair) |
| `Classification` {Public, Restricted, Internal} | apparatus-types | C-04 |
| `Provenance` {source, creator} | apparatus-types | C-05 |
| `ObjectHeader` | apparatus-types | on every object and receipt |
| `Receipt` {header, previous_receipt_id, previous_hash, operation_payload} | apparatus-ledger | the only write path |
| `Sha256Digest`, `derive_cas_path` | apparatus-crypto / -artifacts | Artefact identity |
| `Validate` | apparatus-schema | payload validation |
| `ArtifactStore`, `ReceiptLedger`, `Transaction` | apparatus-store (traits) | persistence |
| `Clock`, `DeterministicClock` | apparatus-time | timestamps; tests |

### X-02 `operation_payload.kind` (v1: 11 kinds)

| Kind | Purpose | Rules served |
|---|---|---|
| `ingest` | Admit/reject an Artefact into canon | C-03, C-05, C-08 |
| `role_bind` | Bind or vacate a role | R-01..R-09 |
| `queue_keep` | Create or move a keep WorkItem | K-05, K-06, K-07, K-08 |
| `queue_build` | Create a build WorkItem (state `proposed`) | B-03 |
| `gate` | G1–G4 decisions on build items; carries outcome | B-02..B-10 |
| `means` | Move means between states or periods | M-01..M-10 |
| `advice` | Record Advice with producer and addressee | A-01, A-05, A-06 |
| `policy` | Adopt rule / agreement / envelope / framework | O-04, K-01, A-02, A-03 |
| `event` | Record a fact, including every failure | §9, K-09, K-11, T-* |
| `correction` | Correct or lawfully remove an earlier object | C-06 |
| `review` | Steps of a scheduled or triggered review | V-01..V-05 |

Adding a kind is a change to this file (§10). Projects MUST NOT invent kinds.

### X-03 Common payload fields

Every payload carries:

```json
{
  "rws": "1.0",
  "kind": "<one of X-02>",
  "subject_id": "<ObjectId the receipt is about>",
  "signer": "<PrincipalId>",
  "role": "continuity | policy | execution | lead | adviser | evaluator | mandate_holder | coordinator",
  "authority_ref": "<ReceiptId of the Policy or gate that authorises this, or null for bootstrap>",
  "evidence": ["<ArtifactId>", "..."],
  "self_certified": false
}
```

Kind-specific fields are listed in the `Compile:` lines of §3–§11 and consolidated in `RWS-2.0-MAPPING.md`.

### X-04 Canonical serialisation

`Receipt::compute_hash` currently hashes `serde_json::to_vec` output, which does not guarantee key order for maps. Before receipts are persisted:

- serialise `operation_payload` with sorted keys and no insignificant whitespace;
- reject payloads containing floating-point money (use integer minor units + `unit`).

Compile: canonical JSON encoder in `apparatus-ledger` (later work, see MAPPING); until then, payload builders MUST emit keys in sorted order.

### X-05 Validator checks (v1 minimum)

Implement as `Validate` impls over the payload, run before `ReceiptLedger::append`:

1. `kind` in X-02.
2. `signer` holds `role` for the scope at `created_at` (R-09).
3. `authority_ref` does not point to Advice or a replica (A-01, A-08).
4. Queue-specific transition table (K-06, B-03).
5. No `gate` on keep items (K-06).
6. `adds_function: true` blocks keep execution (K-07).
7. Means: no spend from `reserved`; no keep↔build moves without `continuity`; no `bound → reserved`; ceiling respected; overprogramming only on build (M-*).
8. Gate evidence non-empty; not only `E`/replica (B-02, C-05).
9. G4 requires keep agreement ref (B-10).
10. `policy` adopting Advice carries `advice_response` (A-03).
11. `policy`/`gate` never signed by an agent principal (R-08, A-05).
12. `Restricted` requires provenance (implemented in `apparatus-schema`).

Every refusal writes `event: illegal_transition_rejected` with the rule ID. A refused receipt is itself never appended.

### X-06 Scheduler duties

The maintenance scheduler (Apparatus later work) emits, without human input:

- `deadline_missed` for review calendar items (V-01);
- `target_missed` (F-03);
- `advice_unanswered` (A-06);
- `means_not_released` (M-05);
- `gate_abandoned` for build items past their review date without a gate (B-04);
- `integrity_failed` when the chain or an artefact hash check fails.

### X-07 CLI surface (v1 target)

Proposed `apparatus` subcommands (only `doctor` exists today):

```text
apparatus rws init          # bootstrap: project, role_bind receipts, empty review calendar
apparatus rws queue keep    # queue_keep receipt
apparatus rws queue build   # queue_build receipt
apparatus rws gate <item> <start|preferred|project|handover> <go|no_go|stop|return> --evidence <artefact>
apparatus rws means <envelope> <from> <to> <amount> --gate <receipt>
apparatus rws event <kind> <subject> --impact "…" --next-role <role> --by <date>
apparatus rws check         # run X-05 over the chain; list open failures and due reviews
```

### X-08 Store assumptions

- v1 designs for SQLite-backed `ReceiptLedger` and a filesystem CAS `ArtifactStore`, as the Apparatus README intends. Neither exists yet.
- RWS 2.0 does not require a policy engine. Role and transition checks are plain validation code.

### X-09 Queues are not tables of their own

Queue membership and state are derived by folding receipts per WorkItem. Any table holding current state is a projection (replica) and can be rebuilt from the chain.

### X-10 Identity of agents

Every agent session that writes Advice or executes a lease gets its own `PrincipalId`. Shared "bot" identities are refused.

### X-11 Repos without a running Apparatus

A greenfield repo often exists before its Apparatus project does. Then:

- Use the frontmatter block (O-11) on governed files as declarations.
- Write receipts, in the exact `Receipt` JSON shape, to one append-only file: `rws/receipts.jsonl`. This file is an **import buffer**, not a ledger: it is imported once into Apparatus with `ingest` receipts referencing each line, then frozen (`rws/receipts.jsonl.imported` marker with the import receipt id).
- Never write to both the buffer and Apparatus for the same project. After import, the buffer is read-only.

Compile: `rws/receipts.jsonl` lines = serialised `Receipt`; import writes `ingest {outcome: admit, purpose: "rws buffer import", source_locator: "rws/receipts.jsonl#L<n>"}` per line.

### X-12 Non-failure event registry (v1)

| Event | Rule |
|---|---|
| `condition_reported` | K-11 |
| `disruption_reported` | K-10, T-06 |
| `report_back`, `report_back_resolved` | A-07 |
| `restore_tested` | T-07 |
| `target_met` | F-03 |
| `stop_recorded` | B-03 |
| `role_vacated` | R-09 |
| `escalated`, `de_escalated` | R-10 |
| `work_started`, `spent`, `deployed`, `contract_signed` | A-04, M-02, B-07 |

Failure events are listed in F-02. Together, F-02 and X-12 are the closed v1 vocabulary.

---

## §13 Attaching RWS 2.0 to a greenfield project (one page)

Do these ten steps, in order, before the first line of product code. Each step leaves a file or a receipt.

1. **Create the project.** `ProjectId` in Apparatus, or `rws/` directory with an empty `receipts.jsonl` (X-11). Record `rws: 1.0` in the repo root README frontmatter.
2. **Bind the triangle.** Three `role_bind` receipts: `continuity`, `policy`, `execution`. Solo owner: same principal, three receipts, `self_certified: true` on the first (R-07). Bind each agent that will work here as its own principal with role `execution` or `adviser` only (R-08).
3. **Name the asset.** One Artefact `rws/asset.md`: what exists (or will exist) that must keep working, its users, its boundaries, its dependencies.
4. **Decide the first queue.** Existing thing that must keep running → `keep` (go to 5). New function → `build` (go to 7). Both → do both; they stay separate.
5. **Keep: write the floor and the agreement.** `rws/floor.md` with indicators under the five criteria (T-02). `rws/agreement.md` with scope, period, envelope, refresh rule, disruption budget or `null` (K-01, K-10). One `policy` receipt, three signers.
6. **Keep: write the first programme.** `rws/programme.md` with firm / planned / outlook bands, ranking, and an explicit `displaced: []` (K-02, K-08, K-09).
7. **Build: open the item.** `queue_build` receipt; `rws/build/<slug>/start.md` start document with problem, scope, most likely solution, build + lifecycle cost with uncertainty, funding sight (B-02, B-08, M-06). Take G1 when ready. Nothing is written in `src/` for this item before G3 unless it is an exploration prototype paid from `committed` exploration means.
8. **Create envelopes.** One per queue per means kind that matters (money, hours, compute). `policy` receipt `sub_kind: envelope`, signed `continuity` (O-09). Keep: `overprogramming: false`.
9. **Set the review calendar.** `rws/reviews.md` with rhythms and next dates for: programme refresh, condition report, account, source audit, policy evaluation, quality review, strategic re-weighing, restore test (V-01, T-07).
10. **Register sources and replicas.** `rws/sources.md`: one authoritative source per shared field (A-07). Mark every dashboard, index, README status table and generated doc as `replica: true` with its sources (A-08).

**Done when:** a stranger can open the repo and answer, from files and receipts only: who holds each role; what must keep working and to what floor; which build items exist and at which gate; how much means sits in each state; which failures are open; when the next review is.

Minimum file set:

```text
rws/
├── receipts.jsonl        # import buffer until Apparatus runs (X-11)
├── asset.md
├── floor.md              # keep only
├── agreement.md          # keep only
├── programme.md          # keep only
├── envelopes.md
├── reviews.md
├── sources.md
└── build/<slug>/{start.md, exploration.md, dossier.md, handover.md}
```

---

## §14 What v1 explicitly does not contain

Left out on purpose. Each item has a reason; open questions are in `OPEN.md`.

| Not in v1 | Why |
|---|---|
| Policy evaluator (e.g. Cedar) | Apparatus has deferred it. v1 enforces by validation code (X-05). |
| Full crisis procedure (levels, teams, timings) | Only the authority rule is kept (R-10). Level definitions are regional practice, not law, in the corpus (G0-5). |
| Work windows with fixed hours | G2-2 found no literal window durations in A/B sources. Only the disruption budget (K-10) is kept. |
| Reserve capacity as a general principle | Only the G3 check (B-13) is kept; A/B corpus did not confirm it. |
| Proportionality levels 0–3 (1.0) | Replaced by the entry rule: only queued WorkItems are governed; small items ride programmes (B-12). Levels were a prose scale, not an object. |
| Speling (bounded margin) as a general permission | Replaced by explicit objects: timing shifts (M-07), programme admission (B-12), gate `return` (B-06). A general "margin" permission is not compilable. |
| Contract management / systems engineering method | G2-2 shows system-based contract control and SE guides exist, but they govern supplier relations inside execution, not this policy. |
| Network zones, MCP surfaces, capability grants, leases | Runtime and security design belong to Apparatus, not to RWS 2.0. |
| Hardware, models, vendors, networks | Execution choices inside build items (P-11). |
| Multi-organisation federation | One project, one chain. Cross-project exchange is later Apparatus work. |
| Numeric defaults copied from the corpus | Corpus numbers appear only in Example boxes. Projects set their own parameters. |
| Named institutions in code | P-10. |
| Sensitivity scale beyond three classes | Apparatus has three. The 1.0 five-level scale (S0–S4) is not mapped; see `OPEN.md`. |

---

## Source IDs used in this file

| ID | Document | Tag |
|---|---|---|
| G1-2-D01 | Spelregels MIRT 2022 (leerplatformmirt.nl PDF) | A, verified |
| G1-2-D05 | Wet Mobiliteitsfonds (BWBR0044860) | A |
| G1-2-D09 | Aanbieding MIRT Overzicht 2027 + DP2027, 15-09-2026 (EK PDF) | A, verified |
| G1-2-D10 | MvT Ontwerpbegroting 2026 Mobiliteitsfonds | A, verified |
| G1-2-D15 | Kamerstuk 36 800 A/J nr. 39, 16-03-2026 | A |
| G1-1-D08 | Kamerstuk 29 385 nr. 142, 28-04-2025 | A |
| G1-1-D10 | Meerjarenplan Instandhouding RWS 2025–2030 | A |
| G1-1-D12 | Kamerstuk 36 800 A nr. 9, 08-12-2025 | A |
| G1-1-D15 | Regeling agentschappen 2024 (BWBR0050264) | A, verified |
| G1-1-D21 | Aanhangsel Handelingen 2025–2026 nr. 682 | A |
| G1-3-D02 | = G1-2-D09 | A, verified |
| G2-1-D07 | Kamerbrief Afweegproces prioritering en perspectief, 19-06-2026 | A, verified |
| G2-2-D02 | Kamerstuk 35 925 A nr. 25, 17-12-2021 | A |
| G2-3-D14 | deltaprogramma.nl/themas/waterveiligheid | B, verified |
| G0-1 … G0-6 | Stage-0 tracks via STAGE-0-UNIFIED.md | A/B as tagged there |

Full URLs: `NOTES/`, `WORKLOG.md` §verification, and the brondossiers in the repo root.
