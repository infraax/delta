# RWS 2.0 — Glossary

Dutch mechanism → RWS 2.0 object or rule. The right-hand column is the only name allowed in code, schemas and APIs (P-10). The Dutch term is for reading the corpus, never for naming things.

Source column: where the mechanism is documented in the corpus. `[V]` = sentence verified against the official source on 2026-09-30.

## Roles and authority

| Dutch term | Mechanism | RWS 2.0 name | Rule | Source |
|---|---|---|---|---|
| continuïteitsverantwoordelijke | SG: supervises policy and general course of the agency; approves budget, plan, accounts; guards equity limits | role `continuity` | R-01 | Regeling agentschappen 2024 art. 1, 6 [V] |
| beleidsverantwoordelijke | DG/director requesting products or services from the agency | role `policy` | R-01 | idem [V] |
| eindverantwoordelijke binnen het agentschap | Highest official in the agency; executes, manages budget, accounts | role `execution` | R-01 | idem [V] |
| eigenaar / opdrachtgever / opdrachtnemer | Pre-2025 names of the same triangle | (retired names; do not use) | R-06 | Stcrt. 2024, 32572 |
| driehoek | The three roles as one governing unit | role triangle | R-02 | G1-1 |
| werkafspraken (≥3 kalenderjaren) | Multi-year work agreements between the roles | `agreement` horizon ≥ 3 periods | R-04 | Regeling art. 7 lid 2 [V] |
| trekker | Named lead responsible for correct application of the rules to one trajectory | `lead` on WorkItem | O-01 | Spelregels MIRT 2022 §1 [V] |
| bevoegd gezag | Authority competent for the legal act | holder of `policy` for the unit; performs the external act (B-07) | B-02, B-07 | Spelregels §2.7 [V] |
| gemandateerde Directeur-Generaal | Official mandated to sign the handover decision | `mandate_holder` | B-02, B-10 | Spelregels §5.5 [V] |
| deltacommissaris | Government commissioner; proposes yearly, monitors, advises; not a minister | role `adviser` | R-05, A-01 | Waterwet 3.6a–b (G1-3) |
| Algemene Rekenkamer / ILT / OvV / Kadaster-audit | Independent evaluators whose findings require a response | role `evaluator` | R-05, A-06 | G0-1, G0-4, G0-5 |
| CBS-onafhankelijkheid (art. 18) | Measurer decides method and publication moment | measurement independence | T-05 | G0-6 |
| GRIP-opschaling | Escalation adds coordination teams; creates no new powers; no upward transfer | `coordinator` role + `escalated` event | R-10 | G0-5 |
| trekker + BO-MIRT "bij voorkeur" | Consultation table prepares; competent authority decides | Advice (table) vs gate (signer) | A-01, B-07 | G2-3 |

## Pipelines and states

| Dutch term | Mechanism | RWS 2.0 name | Rule | Source |
|---|---|---|---|---|
| instandhouding (beheer, onderhoud, vernieuwing) | Keeping existing infrastructure working | queue `keep` | §4 | G1-1 |
| "Voor overige instandhoudingsopgaven gelden de spelregels niet." | Keep is exempt from the build gates | no `gate` receipts on keep | K-06 | Spelregels p. 5 [V] |
| vervanging met toevoeging van extra functionaliteit | Renewal that adds function falls under the build rules | function test → `scope_reclassified` | K-07 | Spelregels p. 5 [V] |
| meerjarenafspraak (statisch) | Fixed-period agreement bundling performance and budget | `Policy` sub-kind `agreement` | K-01 | G1-1 |
| meerjarenprogramma (voortrollend) | Rolling programme, refreshed yearly | programme `Artefact` | K-02 | G1-1 |
| maakbaar geprogrammeerd / vooruit gepland | Firm vs planned horizon | `firm` / `planned` / `outlook` bands | K-02 | Ah-TK 2025–2026 nr. 682 |
| basiskwaliteitsniveau (BKN) | Minimum service level; basis of the agreement | performance floor (`Policy` rule) | K-03 | G1-1 |
| verdringing / verdringingsreeks | Work pushed out when capacity or money falls short | `displacement` event; `deferred` state | K-09 | G1-1 |
| levensduurverlenging vs vernieuwing | Per-object cost comparison | K-08 rank 3 | K-08 | Afweegkader 19-06-2026 [V] |
| MIRT (Meerjarenprogramma Infrastructuur, Ruimte en Transport) | Programme for new/changed infrastructure | queue `build` | §5 | G1-2 |
| voorbereidingsfase / MIRT-onderzoek | Pre-start understanding of the problem | state `proposed` | B-01 | Spelregels |
| startbeslissing + startdocument | Gate opening exploration | gate `start` (G1) + start document | B-02 | Spelregels §2.7 [V] |
| verkenningsfase + verkenningenrapport | Options study | state `exploring` + exploration report | B-01 | Spelregels §3 |
| voorkeursbeslissing | Gate choosing the preferred option; also taken on no-go | gate `preferred` (G2) | B-02, B-04 | Spelregels §3.5 [V] |
| planning- en studiefase | Design and validation | state `designing` | B-01 | Spelregels §4 |
| projectbeslissing | Gate committing to realisation | gate `project` (G3) | B-02 | Spelregels §4.5 [V] |
| aanlegfase | Construction | state `realising` | B-01 | Spelregels §5 |
| opleveringsbeslissing + opleveringsrapportage | Gate accepting delivery, with report | gate `handover` (G4) + handover report | B-02, B-11 | Spelregels §5.5 [V] |
| decharge | Release of the execution organisation from delivery accountability | `discharge` in G4 payload | B-10 | Spelregels §5.5 [V] |
| geen automatische doorstroming / go-no go | No phase change without explicit decision | B-05 | B-05 | Spelregels §1 [V] |
| besluit in juridische zin | Legal act separate from the administrative decision | external act with `gate_ref` | B-07 | Spelregels §1 [V] |
| informatieprofiel | Required content per phase | evidence artefact per gate | B-02, B-08 | Spelregels |
| programmafinanciering | Funding fixed for a programme; components need no separate threshold | programme WorkItem with `admission_rule` | B-12 | Spelregels §2.6 |
| bandbreedte kostenraming / overschrijding binnen projectbudget | Estimate ranges; overruns absorbed first | `design_uncertainty`; `envelope_overrun` | B-08, B-09 | Spelregels; 36 800 A/J nr. 39 |

## Means

| Dutch term | Mechanism | RWS 2.0 name | Rule | Source |
|---|---|---|---|---|
| juridisch verplicht | Contracted; includes instandhouding, DBFM, realisation, apparatus | means state `bound` | M-01, M-03 | MvT MF 2026 [V] |
| bestuurlijk gebonden | Administratively promised (exploration, planning) | means state `committed` | M-01 | MvT MF 2026 [V] |
| beleidsmatig gereserveerd | Policy reservation | means state `reserved` | M-01 | MvT MF 2026 [V] |
| flexnorm / amendeerbaar | Committed-not-bound remains re-decidable | M-04 | M-04 | MvT MF 2026 §2.4 [V] |
| overheveling naar productartikel | After preferred decision, budget moves to the item's own line | `reserved → committed` at G2 | M-05 | MvT MF 2026 art. 11 [V] |
| gereserveerde middelen vallen vrij | No-go releases reservation | means release on `no_go` | M-05 | Spelregels §3.5 [V] |
| 75%-regel / zicht op financiering | Funding-sight threshold at start | `start_funding_share` | M-06 | Spelregels §2.6 [V] |
| 100% financiële middelen | Full funding at preferred decision | G2 means threshold | B-02 | Spelregels §3.5 [V] |
| kasschuif (budgetneutraal) | Moving cash between years without changing total | `timing_shift` | M-07 | MvT MF 2026 [V] |
| kaderruil | Swap between lines, reversed later, net zero | `timing_shift` between lines | M-07 | MvT MF 2026 tabel 7 |
| eindejaarsmarge (100%) | Carry-over of unspent funds | Envelope `carry_over` | M-08 | MvT MF 2026 [V] |
| overprogrammering (niet op onderhoud) | Planning above ceiling for slip-prone build only | Envelope `overprogramming` | M-09, K-04 | MvT MF 2026 §2.3 [V] |
| uitgavenplafond | Ceiling on total available budget | Envelope `ceiling` | O-09 | MvT MF 2026 [V] |
| begrotingsfonds (ring-fenced) | Fund whose money is legally tied to purpose | Envelope per queue; no keep↔build moves | K-04, M-11 | Wet MF; Waterwet 7.22a |
| uitsluiting bij juridische verplichtingen | Bound items excluded from re-prioritisation | M-10 | M-10 | Afweegkader 19-06-2026 [V] |
| maakbaarheid > financieel kader | Capacity exceeds money | `capacity_exceeds_envelope` | F-02 | 36 800 A nr. 9 |

## Advice, policy, replica, canon

| Dutch term | Mechanism | RWS 2.0 name | Rule | Source |
|---|---|---|---|---|
| "DP adviserend, NWP legt het beleid vast" | Programme advises; separate instrument fixes policy | `Advice` vs `Policy` | A-01, A-02 | deltaprogramma.nl [V] |
| kabinetsreactie | Adopting role's documented response to advice | `policy` receipt with `advice_response` | A-03 | EK brief 15-09-2026 [V] |
| rekening houden met advies (Waterwet 4.9 lid 7) | State how advice was used | `advice_response` | A-03 | G1-3 |
| afweegkader → herleidbare prioritering | Framework adopted before ranking | `framework` Policy + ranking Advice | A-09 | Afweegkader 19-06-2026 [V] |
| MIRT Overzicht = naslagwerk / bijstuk | Yearly reference overview | replica | A-08 | Spelregels [V]; EK brief [V] |
| "Aan dit overzicht … kunnen geen rechten worden ontleend." | Overview grants no rights | replica disclaimer | A-08 | MIRT Overzicht 2027 [V] |
| authentiek gegeven | One legally designated source per data field | authoritative source | A-07 | G0-4, G0-6 |
| terugmelden | Report suspected error to source holder | `report_back` event | A-07 | G0-4, G0-6 |
| NIET GEVONDEN / [NF] | Absence kept as a row | `not_found` event | C-08 | INDEX-WORK-FROM-TRACKS |
| [A] / [B] / [C] tags | Evidence strength | `evidence_tag` | C-05 | all dossiers |
| correctieprotocol (CBS) | Open correction of published output | `correction` receipt | C-06, T-05 | G0-6 |

## Failure, review, measurement

| Dutch term | Mechanism | RWS 2.0 name | Rule | Source |
|---|---|---|---|---|
| "niet meer haalbaar" (ISO-doel) | Target abandoned | `target_dropped` | F-02, F-03 | 29 385 nr. 142 |
| evaluatieverslag binnen vijf jaar | Statutory evaluation deadline | review calendar `policy evaluation`; `deadline_missed` | V-01 | Wet MF art. 9 |
| externe audit ≥ 1× per 3 jaar (BRK) | Periodic source audit | review calendar `source audit` | V-01 | G0-4 |
| zesjaarlijkse herijking | Strategic re-weighing, six steps | V-02 | V-02 | G0-3, G1-3 |
| herijkt, geen aanleiding tot wijziging | "No change" as recorded outcome | `reaffirms` | V-03 | deltaprogramma.nl [V] |
| Staat van de Infrastructuur (5 criteria) | Periodic condition report | `condition_reported`; T-02 criteria | K-11, T-02 | G0-1, G2-2 |
| indicatoren niet gebruikt voor aansturing | Measured but not steering | `steering: false` | T-03 | G0-1 |
| compleet overzicht beperkingen | Constraint register | T-04 | T-04 | G0-1 |
| hinder / 10%-filedrukafspraak | Disruption caused by work, capped | disruption budget | K-10 | G2-2 |
| kort en hevig | Short full closure over long partial | K-10 preference | K-10 | G2-2 |
| OvV-aanbeveling + verplichte reactie | Findings must be answered | `advice_unanswered` | A-06 | G0-5 |

## Terms from 1.0 documents

| 1.0 term | Status in RWS 2.0 |
|---|---|
| Asset Owner / Asset Manager / Service Provider | Replaced by `continuity` / `policy` / `execution` |
| Nation | Dropped (metaphor) |
| Canon | Kept: Artefacts + Receipts of a project (§8) |
| Speling | Replaced by explicit objects (M-07, B-12, B-06); see §14 |
| Levels 0–3 | Replaced by queue entry rule + programmes (B-12) |
| Terrain Map, Decision Calculation | Content required in start document and exploration report (B-02, B-08) |
| Reserve-capacity assessment | Kept as G3 check (B-13) |
| Handover record | Kept as G4 evidence (B-10, B-11) |
| Sluiproute | Covered by B-07 (`gate_bypassed`) and C-02 (one ledger) |
| Module permit | Apparatus concern; not in RWS 2.0 |
