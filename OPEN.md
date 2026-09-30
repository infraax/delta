# OPEN — RWS 2.0 v1

What is not found, what is in tension, and what v1 leaves out on purpose. Every item names where it came from and what would close it.

## 1. Missing inputs (in this repo)

| # | Item | Status | Effect on v1 |
|---|---|---|---|
| 1.1 | v0 field manual (`RWS-2.0.md` v0 / `blueprint/`) | Not in `infraax/delta` or `infraax/Aparatus`. | Frozen core taken from the session brief §1. If v0 surfaces, diff it against §0 of RWS-2.0.md; the frozen core must match. |
| 1.2 | Stage-1 unified file | Not present. | Replaced by reading G1-1, G1-2, G1-3 in full. |
| 1.3 | Stage-2 unified file | Not present. | Replaced by reading G2-1, G2-2, G2-3 in full. |
| 1.4 | `QUOTES-AND-DEFINITION.md` | Not present. | No unverified-quote appendix imported. All epigraphs carry a source ID. |
| 1.5 | PARKING report (`PARKING-Plan-Dutch-Infrastructure-Research.pdf`) cited by 1.0 docs | Not present. | 1.0 claims sourced only to PARKING (night/weekend windows, reserve pipes, "20.000 meetpunten") are not treated as corpus evidence. |
| 1.6 | Footnote dump `output.md` ("bestand 2") | Not present. | Index tables for G0-4/G0-5 remain partial. |

## 2. Corpus gaps [NF] that touch rules

| # | Gap | Source | Rule affected | What would close it |
|---|---|---|---|---|
| 2.1 | Text and signers of the keep agreement (meerjarenafspraak 2024–2030 / Startbrief nov 2023) | G1-1 | K-01 (signer set is inferred from Regeling art. 6, not from the agreement itself) | Woo request or beslisnota bij 29 385 nr. 139 |
| 2.2 | Midterm review 2026 of the agreement | G1-1 | V-05 | 29 385 nr. 146 text |
| 2.3 | BKN norm table (bijlage 1081483) | G1-1, G2-1 | K-03 indicator example | Open the annex PDF |
| 2.4 | SAMP v3 (internal) | G1-1 | none directly; would inform floor→strategy link | Not public |
| 2.5 | Replacement date for dropped ISO target | G1-1 | F-03 example only | Letter announced in 36 800 A nr. 9 |
| 2.6 | Evaluation Wet Mobiliteitsfonds art. 9 (due 1 Jul 2026) | G1-2 | V-01 example (`deadline_missed`) | Jaarverslag MF 2025; toezeggingenlijst |
| 2.7 | Fate of the 75% rule under the "herziening MIRT-systematiek" (end 2026) | G1-2 | M-06 is a parameter, so v1 is unaffected | MIRT-brief autumn 2026 |
| 2.8 | Verb in kabinetsreactie DP2027 (vaststellen / overnemen / kennisnemen) | G1-3, G2-1 | A-02 wording of adoption | Kamerstuk 37 020-J nr. 4 text |
| 2.9 | Legal duty for six-yearly re-weighing | G1-3 | V-02 is programme practice, not law; v1 treats it as a calendar default | Consolidated Waterwet 2026 |
| 2.10 | Literal work-window durations; reserve-capacity rule; ILS/clash; BIM status | G2-2 | K-10 kept as budget only; B-13 kept from 1.0 only | RWS werkwijzer hinderaanpak PDF; Kader Contractbeheersing |
| 2.11 | Whether 60-min / 10-min / 75%-satisfaction hinder norms were adopted | G2-2 | K-10 example only | Rapportage Rijkswegennet |
| 2.12 | "Bekrachtiging" of deltabeslissingen by BO Water / stuurgroep in an A/B source | G1-3, G2-3 | none; the adopt/advise split (A-01) rests on verified B source | UvW or IPLO primary text |
| 2.13 | Field→article matrix for authentic data | G0-4, G0-6 | A-07 is generic, not dependent | Per-registration law texts |
| 2.14 | Public mandate map in the new role names | G1-1 | R-06 (names only via binding) unaffected | — |

## 3. Tensions with the frozen core (recorded, core unchanged)

| # | Tension | Where | Handling in v1 |
|---|---|---|---|
| 3.1 | 1.0 Operating Method runs maintenance through the same universal route and gates as new work. | Aparatus `The Dutch Way operating method.md` §1, §17 | Frozen core wins: keep has no build gates (K-06). 1.0's gate content is reused only for build. |
| 3.2 | The keep agreement is called "statisch" in the programme text but "voortrollend" by the minister in debate. | G1-1 §5.3 | Modelled as two objects: static agreement (K-01) + rolling programme (K-02). Matches the MJP wording [A]. |
| 3.3 | Instandhouding counts as "juridisch verplicht" in the fund's flexibility table, yet the afweegkader of June 2026 puts exploitation, maintenance and renewal inside the re-prioritisation scope. | G1-2 vs G2-1 | Keep envelope is `bound` for the agreement period (M-03); only items whose means are actually contracted are excluded from re-prioritisation (M-10). Keep items not yet contracted can be re-ranked at a portfolio review (V-07) but never moved into build envelopes. Watch the autumn 2026 prioritisation outcome. |
| 3.4 | Session brief says "handover-receipt door mandated executor"; the corpus has the handover signed by a mandated DG (policy side) who grants discharge to the execution organisation. | Brief §3.4 vs Spelregels §5.5 [V] | Refined, not reversed: G4 is signed by `mandate_holder` on the policy side; it discharges `execution`. An executor signing its own discharge is forbidden (B-14.7). |
| 3.5 | Brief suggested `queue_keep` / `queue_build` as receipt kinds; a queue is also an object (O-08). | Brief §5.3 | Both kept: Queue is a fixed state-machine definition; the receipts move items through it. |
| 3.6 | Brief suggests "reserved → committed; only then spend API". Corpus shows exploration budget becomes "bestuurlijk gebonden" at start, and full budget moves to the item's line at the preferred decision. | MvT MF 2026 [V] | M-05: exploration means committed at G1; full-option means committed at G2; bound after contracts following G3. Spend only from committed/bound (M-02). Consistent with the brief. |
| 3.7 | X-11 import buffer `rws/receipts.jsonl` could be read as a second ledger. | RWS-2.0 X-11 vs brief §2 | Buffer is single-use, imported then frozen; never concurrent with an Apparatus chain. If the owner prefers no buffer at all, the alternative is: no receipts until Apparatus runs, frontmatter only. Decision for the owner. |
| 3.8 | Agents barred from `policy`/`continuity` (R-08) is an RWS 2.0 choice, not a corpus finding. | RWS-2.0 R-08 | Derived from Advice ≠ Policy (frozen core 4) and Apparatus design law 7. Keep unless owner decides otherwise. |

## 4. Deliberate omissions from v1 (1.0 material)

| # | 1.0 element | Where | Why left out | Revisit when |
|---|---|---|---|---|
| 4.1 | Speling (bounded margin) as a general permission | Constitution §14 | Not compilable as one object. Its legitimate uses are covered by timing shifts (M-07), programme admission (B-12), gate `return` (B-06), overrun absorption (B-09). | If a real case needs a margin not covered by these. |
| 4.2 | Proportionality levels 0–3 | Constitution §12, Field Manual §1 | A prose scale. v1 governs only queued WorkItems; small items go under programmes (B-12). | If projects drown in gates for trivial work. |
| 4.3 | Priority groups I/II/III (purpose/reliability/resilience; economy>quality>speed; protected considerations) | Constitution §4 | Values ranking, not an object. K-08 ranking and V-07 framework carry the operational part; the framework Policy of a project may embed this ordering. | Owner may adopt it as the default `framework` Policy. |
| 4.4 | Field Manual templates (Terrain Map, Decision Calculation, Lifecycle Cost Register, etc.) | Field Manual §3–§14 | Their required content is folded into gate evidence (B-02, B-08, B-11); the templates themselves are not policy. | Could ship as `rws-2-0/templates/` in a later skill version. |
| 4.5 | Sensitivity scale S0–S4 and reliability tiers A–E | Corpus Protocol §7–§8 | Apparatus has 3 classes. Tiers mapped to `evidence_tag` (A/B/C/E/NF); S-scale not mapped. | When Apparatus extends `Classification`. |
| 4.6 | Night/weekend/holiday maintenance windows | RWS-Blueprint §7; Field Manual §14 | Sourced to PARKING (not in repo); A/B corpus found no literal durations (G2-2). Only disruption budget kept (K-10). | If G2-2 follow-up finds the werkwijzer text. |
| 4.7 | Module permit system, event envelope fields, widget lanes, iOS re-sign | RWS-Blueprint | Runtime/product specifics; brief forbids. | — |
| 4.8 | Network zones, MCP servers, capability grants, leases, model router | The Apparatus / Design.md | Apparatus design, not RWS policy. | — |
| 4.9 | Cedar policy engine | Design.md ADR-0001 | Deferred in Apparatus; brief forbids as v1 requirement. | After Apparatus M1 spike. |
| 4.10 | Crisis level definitions (GRIP 1–5 teams) | G0-5 | Regional practice, not law. Only the authority rule kept (R-10). | If projects need multi-level incident procedure. |

## 5. Duplicates in the repos (recorded, not changed)

- `delta/G0-1.md` == `delta/GO-02.md.md` (content: G0-2 track). The file named `G0-1.md` does not contain G0-1.
- `delta/g0-3.md.md` == `delta/g0-3-deltaprogramma.md.md`.
- `Aparatus/The Dutch Way.md` == `Aparatus/The Dutch Way Corpus protocol.md`.
- `Aparatus/Design.md` §1–27 duplicates `The Apparatus.md`.
- Suggested cleanup (owner decision): rename `G0-1.md` → delete or `GO-02` alias; keep one copy of each duplicate.

## 6. Owner decisions requested

1. Accept or replace the import buffer (3.7).
2. Adopt Constitution priority groups as the default `framework` Policy (4.3) — yes/no.
3. Confirm R-08 (agents never hold `policy`/`continuity`).
4. Authorise the Apparatus code changes listed in `RWS-2.0-MAPPING.md` §3.
5. Commit this set (proposed message in chat).
