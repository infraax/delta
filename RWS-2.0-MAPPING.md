# RWS 2.0 — Mapping to Apparatus

Which Apparatus crate or type carries each rule, and whether v1 implements it now or later.

Baseline: `infraax/Aparatus` @ `095d1e1` (M0 scaffold). Present crates: `apparatus-types`, `-time`, `-crypto`, `-schema`, `-store` (traits only), `-artifacts` (CAS path helpers), `-ledger` (Receipt + hash), `apparatus-cli` (`doctor` only).

Legend:

- **v1** — implement in the next Apparatus milestone; needs only present crates plus new code inside them.
- **v1-file** — enforceable today through frontmatter + `rws/receipts.jsonl` import buffer (RWS-2.0 X-11) and a checker script; no runtime needed.
- **later** — needs a crate that does not exist yet (registry, scheduler, gateway, policy evaluator) or storage that is not built (SQLite ledger, filesystem CAS).

## 1. Section → crate/type → status

| Manual section | Rule(s) | Apparatus crate / type | Status |
|---|---|---|---|
| §0 Frozen core | all | — (policy) | — |
| §2 Object header | O-01..O-09 | `apparatus-types::ObjectHeader`, `ObjectId` | v1 (present) |
| §2 WorkItem | O-01 | new struct in `apparatus-types` (or payload-only, folded from receipts, X-09) | v1 |
| §2 Artefact | O-02 | `apparatus-artifacts::ArtifactDescriptor`, `derive_cas_path`; `apparatus-crypto::Sha256Digest` | v1 (present helpers); CAS write later |
| §2 Advice / Policy / Event | O-03..O-05 | `Receipt.operation_payload.kind` = `advice` / `policy` / `event` | v1 |
| §2 Receipt | O-06 | `apparatus-ledger::Receipt` | v1 (present) |
| §2 Role | O-07 | `PrincipalId` + `role_bind` payload | v1 |
| §2 Queue | O-08 | `queue_keep` / `queue_build` payload + transition tables in `apparatus-schema` | v1 |
| §2 Envelope | O-09 | new struct in `apparatus-types`; `policy` + `means` payloads | v1 |
| §2 Frontmatter | O-11 | markdown frontmatter `rws:` block | v1-file |
| §3 Triangle + binding | R-01..R-04, R-09 | `role_bind`; check in `apparatus-schema` (`signer` holds `role` at `created_at`) | v1 |
| §3 Naming rule | R-06 | lint over schemas/code (no institutional names) | v1-file |
| §3 Solo mode | R-07 | payload `self_certified` | v1 |
| §3 Agents | R-08 | `PrincipalId` per agent; `producer_kind` | v1 |
| §3 Escalation | R-10 | `event: escalated` + `role_bind {role: coordinator}` | v1 |
| §4 Agreement | K-01 | `policy {sub_kind: agreement}` with correlated signers (`CorrelationId`) | v1 |
| §4 Programme | K-02 | Artefact + `policy {sub_kind: rule}` approval | v1 (record); refresh scheduling later |
| §4 Floor | K-03 | `policy {sub_kind: rule, indicators}` | v1 |
| §4 Keep envelope | K-04 | Envelope `{queue: keep, overprogramming: false}` | v1 |
| §4 Keep states/transitions | K-05, K-06 | `queue_keep` + table in `apparatus-schema` | v1 |
| §4 Function test | K-07 | WorkItem `adds_function`; `event: scope_reclassified` | v1 |
| §4 Priority | K-08 | programme Artefact `ranking[]`; payload `urgent` | v1-file |
| §4 Displacement | K-09 | `event: displacement`; programme `displaced[]` | v1 |
| §4 Disruption budget | K-10 | agreement field; `event: disruption_reported` | v1 (record); measurement later |
| §4 Condition report | K-11 | `event: condition_reported` | v1 |
| §4 Forbidden keep ops | K-12 | validator refusals → `event: illegal_transition_rejected` | v1 |
| §5 Gates | B-01..B-06 | `gate` payload + transition table | v1 |
| §5 Gate vs act | B-07 | `gate_ref` on act receipts; `gate_bypassed` | v1 (inside Apparatus); detection of outside acts later |
| §5 Evidence quality | B-08 | frontmatter `estimates[]`; warn in checker | v1-file |
| §5 Overrun | B-09 | `event: envelope_overrun`; `means` refusal | v1 |
| §5 Handover/discharge | B-10, B-11 | G4 payload `discharge`, `keep` | v1 |
| §5 Programmes | B-12 | WorkItem `programme_ref`; Policy `admission_rule` | v1 |
| §5 Reserve capacity | B-13 | dossier field `reserve_capacity[]` | v1-file |
| §6 Means states | M-01..M-04 | Envelope buckets; `means` payload | v1 |
| §6 Movement by gate | M-05 | `means.gate_ref` / `agreement_ref` checks | v1 |
| §6 Start threshold | M-06 | project Policy `start_funding_share`; G1 `means_check` | v1 |
| §6 Timing shift | M-07 | `means.timing_shift`; multi-period sum check | v1 |
| §6 Carry-over | M-08 | Envelope `carry_over` | v1 (declared); period-close job later |
| §6 Overprogramming | M-09 | Envelope `overprogramming` | v1 |
| §6 Bound excluded | M-10 | re-prioritisation Artefact; refusal `bound → reserved` | v1 |
| §7 Advice ≠ policy | A-01..A-05 | `authority_ref` type check; `producer_kind` | v1 |
| §7 Evaluator response | A-06 | `advice {addressed_to, respond_by}`; `advice_unanswered` | v1 (record); scheduler emission later |
| §7 Authoritative source | A-07 | registry Artefact; `report_back` events | v1-file; registry crate later |
| §7 Replica | A-08 | Artefact frontmatter `replica: true`; evidence check | v1 |
| §7 Framework first | A-09 | `framework_ref` check | v1 |
| §8 Append-only | C-01 | `ArtifactStore::store` fails on existing id; `ReceiptLedger::append` | v1 (trait contract present); impl later (SQLite/CAS) |
| §8 One ledger | C-02 | one chain head per `ProjectId` | later (needs ledger impl); v1-file buffer meanwhile |
| §8 Ingest | C-03 | `ingest` payload; `ArtifactDescriptor` | v1 |
| §8 Classification | C-04 | `Classification`; `Validate for ObjectHeader` | v1 (present) |
| §8 Provenance + tags | C-05 | `Provenance`; payload `evidence_tag` | v1 |
| §8 Corrections | C-06 | `correction` payload | v1 |
| §8 Time | C-07 | `apparatus-time::Clock`; payload `occurred_at` | v1 (present) |
| §8 Not found | C-08 | `event: not_found` | v1 |
| §9 Failure shape/kinds | F-01, F-02 | `event` payload; closed vocabulary with X-12 | v1 |
| §9 No silent drop | F-03 | Policy `targets[]`; `target_missed` emission | v1 (record); scheduler later |
| §9 Failure → review | F-05 | review Artefact `open_failures[]` | v1-file |
| §10 Calendar | V-01 | Policy `review_calendar[]`; `deadline_missed` | v1 (record); scheduler later |
| §10 Re-weighing procedure | V-02, V-03 | `review` payload steps; `reaffirms` | v1 |
| §10 Triggers | V-04, V-05 | `review.trigger.ref` check | v1 |
| §10 Horizon | V-06 | programme `horizon` check | v1-file |
| §10 Portfolio re-prioritisation | V-07 | re-prioritisation Artefact | v1-file |
| §11 Telemetry | T-01..T-07 | `ingest {evidence_tag: A}`; replicas; `restore_tested` | v1 (record); collectors later |
| §12 Canonical JSON | X-04 | `apparatus-ledger::Receipt::compute_hash` needs canonical encoder | v1 (must fix before persistence) |
| §12 Validator | X-05 | `apparatus-schema::Validate` impls per payload kind | v1 |
| §12 Scheduler | X-06 | new `apparatus-maintenance` crate | later |
| §12 CLI | X-07 | `apparatus-cli` subcommand `rws` | v1 (init/queue/gate/means/event/check) |
| §12 Store | X-08 | `apparatus-store` SQLite + filesystem CAS | later |
| §12 Import buffer | X-11 | `rws/receipts.jsonl` + import command | v1-file now; import command v1 |
| §14 Policy evaluator | — | Cedar behind `PolicyEngine` trait (Design.md ADR-0001) | later; not required by RWS 2.0 |

## 2. `operation_payload.kind` — consolidated fields

All payloads include the common fields (RWS-2.0 X-03): `rws`, `kind`, `subject_id`, `signer`, `role`, `authority_ref`, `evidence[]`, `self_certified`.

| Kind | Required fields | Optional | Validator checks |
|---|---|---|---|
| `ingest` | `outcome` (admit\|admit_restricted\|reference_only\|quarantine\|reject), `purpose`, `digest`, `size_bytes`, `media_type`, `source_locator`, `evidence_tag` (A\|B\|C\|E\|NF) | `artifact_id`, `reason`, `occurred_at`, `replica`, `sources[]` | Restricted ⇒ provenance; reject ⇒ `reason` |
| `role_bind` | `role`, `principal` (nullable = vacate), `scope {project_id, asset_ref?, envelope_ref?, incident_ref?}`, `from` | `until` | signer holds `continuity` (or bootstrap); agent principals only `execution`/`adviser` |
| `queue_keep` | `from` (null on create), `to`, `asset_ref`, `lead` (on create), `programme_ref` | `urgent: safety`, `disruption_class`, `window` | K-06 table; agreement covers asset; `adds_function` false for `executing` |
| `queue_build` | `to: proposed`, `asset_ref`, `lead`, `adds_function: true` | `programme_ref` | create only; later moves via `gate` |
| `gate` | `gate` (start\|preferred\|project\|handover), `outcome` (go\|no_go\|hold\|return\|stop), `signers[] {principal, role}`, `evidence[]`, `means_check {envelope_ref, required, available, share}` | `conditions[]`, `valid_until`, `discharge {role, principal}`, `keep {agreement_ref, envelope_ref, baseline_event_ref}` | B-02 roles; evidence non-empty, not only E/replica; threshold; G4 needs `keep.agreement_ref`; not on keep items |
| `means` | `envelope_ref`, `from_state`, `to_state`, `amount` (integer minor units), `unit` | `item_ref`, `gate_ref`, `agreement_ref`, `act_ref`, `timing_shift {from_period, to_period}` | M-02..M-10 |
| `advice` | `producer`, `producer_kind` (human\|agent\|service\|external_model), `inputs[]` | `addressed_to`, `respond_by`, `framework_ref`, `method_ref`, `published_at` | none may be `authority_ref` elsewhere |
| `policy` | `sub_kind` (rule\|agreement\|envelope\|framework), `body_ref` (ArtifactId), `signers[]` | `adopts[]`, `advice_response[] {advice_id, response, reason}`, `supersedes`, `reaffirms`, `targets[]`, `trigger {type, ref}`, `review_calendar[]`, `start_funding_share`, `restricted_category_added` | roles per sub-kind; `adopts` ⇒ `advice_response`; never signed by agent |
| `event` | `event` (F-02 ∪ X-12), `detected_by`, `detected_at` | `impact`, `next_decision {role, by, options[]}` (mandatory for F-02 kinds), `refs[]` | vocabulary closed |
| `correction` | `corrects`, `reason` | `replacement`, `removal: bool` | corrector role ≥ original signer's role for the scope; evaluator output only by evaluator |
| `review` | `review` (programme_refresh\|condition\|account\|replica\|source_audit\|policy_evaluation\|quality\|strategic), `rhythm` (scheduled\|out_of_cycle) | `step` (1..6), `artefact_ref`, `trigger {type, ref}` | out_of_cycle ⇒ trigger ref exists |

Kind count: 11 (limit ~12). Adding a kind changes RWS 2.0 itself.

## 3. Minimal code changes proposed for Apparatus (not made in this session)

| # | Crate | Change | Reason |
|---|---|---|---|
| 1 | `apparatus-types` | Add `Role`, `QueueKind`, `MeansState`, `GateKind`, `GateOutcome`, `EvidenceTag` enums (serde snake_case) | Typed payloads instead of free strings |
| 2 | `apparatus-types` | Add `Envelope` struct (id, means_kind, period, ceiling, buckets, queue, carry_over, overprogramming) | O-09 |
| 3 | `apparatus-ledger` | Canonical JSON serialisation for `compute_hash` (sorted keys; no floats in payload) | X-04; FINAL_REPORT already flags this risk |
| 4 | `apparatus-schema` | `Validate` impls per payload kind + transition tables for keep/build | X-05 |
| 5 | `apparatus-schema` | Role-at-time check (needs a read view over `role_bind` receipts) | R-09 |
| 6 | `apparatus-cli` | `rws init/queue/gate/means/event/check` | X-07 |
| 7 | new `apparatus-maintenance` | Scheduler emitting X-06 events | later |

## 4. Classification mapping

| RWS 2.0 / corpus | Apparatus `Classification` |
|---|---|
| Public (may leave project) | `Public` |
| Internal operations | `Internal` |
| Personal, secret, legal, security | `Restricted` (provenance mandatory) |
| 1.0 S0–S4 scale | Not mapped in v1 → `OPEN.md` |

## 5. Things RWS 2.0 does not ask of Apparatus

- No policy evaluator (Cedar or other).
- No network zones, MCP facade, capability grants, leases — those remain Apparatus design (Design.md) and are orthogonal.
- No second ledger. The `rws/receipts.jsonl` import buffer exists only until the project's Apparatus chain runs, then is frozen (X-11).
