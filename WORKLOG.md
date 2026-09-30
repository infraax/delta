# WORKLOG — RWS 2.0 session

Session date: 2026-09-30. Operator: Claude Code (remote sandbox). Repo: `infraax/delta` @ `95c04c9`. Runtime reference: `infraax/Aparatus` @ `095d1e1` (sibling checkout `/home/user/Aparatus`).

Format: `# | action | file / URL | opened | decision`.

## Setup

| # | Action | Target | Opened | Decision |
|---|---|---|---|---|
| 1 | clone/pull | `infraax/delta` | yes | Already present, clean, `main`. Corpus present → proceed (§7 stop-rule not triggered). |
| 2 | inventory | all `*.md` in delta | yes | 17 files. No `RWS-2.0.md` v0, no `blueprint/`, no Stage-1 or Stage-2 unified, no `QUOTES-AND-DEFINITION.md`. Logged in OPEN.md. |
| 3 | dedup | md5 over delta + Aparatus | yes | 3 duplicate pairs. See `NOTES/DUPLICATES.md`. Nothing deleted. |
| 4 | inventory | `infraax/Aparatus` | yes | 1.0 documents live here, not in delta. Read from there. |

## Reading order (one file at a time, NOTES after each)

| # | Action | File | Opened | Decision |
|---|---|---|---|---|
| 5 | read code | Aparatus `crates/*/src/lib.rs`, `bins/apparatus-cli/src/main.rs`, `Cargo.toml`, README, FINAL_REPORT, plan_report | yes, full | Type names fixed from source, not from brief. → `NOTES/THE-APPARATUS.md` |
| 6 | read | (v0 `RWS-2.0.md`) | absent | Build on the frozen core stated in the session brief §1. Core not altered. |
| 7 | read | `STAGE-0-UNIFIED.md` | yes, full | → `NOTES/STAGE-0-UNIFIED.md` |
| 8 | read | Stage-1 unified | absent | Replaced by G1-1/G1-2/G1-3 brondossiers (read in full). |
| 9 | read | Stage-2 unified | absent | Replaced by G2-1/G2-2/G2-3 brondossiers (read in full). |
| 10 | read | `INDEX-WORK-FROM-TRACKS.md` | yes, full | → NOTES |
| 11 | read | `STAGE-0-INDEX-1.md.md` | first 6 KB + structure | Register only; used for IDs. → NOTES |
| 12 | read | G0 roots (`G0-1.md.md`, `GO-02.md.md`, `g0-3.md.md`, `GO-04.md`, `G0-5.md.md`, `g0-6.md.md`) | grep-targeted only | Unified covers them; roots consulted for GRIP-no-new-powers, CBS art. 18, authentic-data rule, terugmelding, BRK audit. → NOTES per root |
| 13 | read | `Brondossier G1-1_ …md` | yes, full | Keep assignment mechanics. → `NOTES/G1-1.md` |
| 14 | read | `MIRT-gates … G1-2 …md` | yes, full | Gates, signers, thresholds, means states. → `NOTES/G1-2.md` |
| 15 | read | `Deltaprogramma 2027 … G1-3.md` | yes, full | Advice vs policy, herijking. → `NOTES/G1-3.md` |
| 16 | read | `G2-1 — Ongeopende A-stukken.md` | yes, full | Afweegkader, legal-commitment exclusion. → `NOTES/G2-1.md` |
| 17 | read | `G2-2.md` | yes, full | EXISTS (brief expected missing). Windows/reserve capacity still [NF] inside it. → `NOTES/G2-2.md` |
| 18 | read | `G2-3.md` | yes, full | Tables, HWBP alliance, escalation. → `NOTES/G2-3.md` |
| 19 | read | Aparatus `The Dutch Way Field Manual .md` | yes, full | → NOTES |
| 20 | read | Aparatus `The Dutch way Constitution.md` | yes, full | → NOTES |
| 21 | read | Aparatus `The Dutch Way operating method.md` | yes, full | Keep/build conflation noted → OPEN.md |
| 22 | read | Aparatus `The Dutch Way Corpus protocol.md` (== `The Dutch Way.md`) | yes, full | → NOTES |
| 23 | read | Aparatus `RWS-Blueprint Foundation Architecture.md` | yes, full | → NOTES |
| 24 | read | Aparatus `The Apparatus.md` | yes, full | → NOTES |
| 25 | read | Aparatus `Design.md` | headings + Core §1–3, §8, §10, §18, ADR-0001 §1–3 | Rest duplicates The Apparatus or is Engram (out of scope). |
| 26 | read | `QUOTES-AND-DEFINITION.md` | absent | No quote appendix to import. |

## Official-source verification (A/B only; 6 fetches)

| # | URL | Opened | Confirmed literally |
|---|---|---|---|
| 27 | https://leerplatformmirt.nl/publish/pages/233539/1-mirt-spelregels-2022_1.pdf | yes (PDF text) | "Voor overige instandhoudingsopgaven gelden de spelregels niet."; "Er is geen automatische doorstroming…"; "Er wordt dus ook een voorkeursbeslissing genomen bij een no go."; §2.6 75%; §2.7 start by bevoegd gezag; §3.5 100%; §4.5 project by bewindsperso(o)n(en); §5.5 DG + decharge + no consultation on agreement; "gereserveerde middelen vrij"; "besluit in juridische zin"; "naslagwerk"; ±25% / ±35%; trekker responsible for correct application. |
| 28 | https://www.rijksfinancien.nl/memorie-van-toelichting/2026/OWB/A | yes | "overgeheveld naar het desbetreffende productartikel"; "100% eindejaarsmarge"; "Kasschuiven zijn altijd budgetneutraal"; juridisch verplicht / bestuurlijk gebonden / beleidsmatig gereserveerd; "onverminderd door de Tweede Kamer te amenderen"; no overprogramming on maintenance; "vastgestelde uitgavenplafond". |
| 29 | https://wetten.overheid.nl/BWBR0050264/2025-01-01/0 | yes | Art. 6 lid 1–3 (three roles, joint setting of activities, end-responsible duties); art. 7 lid 2 (≥3 calendar years); role definitions. |
| 30 | https://www.deltaprogramma.nl/themas/waterveiligheid | yes | "Het Deltaprogramma is daarin adviserend, het Nationaal Water Programma legt het beleid vast."; "geen aanleiding … de doelstellingen of de systematiek te wijzigen". |
| 31 | https://www.tweedekamer.nl/kamerstukken/brieven_regering/detail?id=2026Z13948&did=2026D31270 | yes | legal-commitment exclusion; afweegkader adopted after debate; "levensduurverlenging als het kan…"; "minimaal acht jaar". |
| 32 | https://www.eerstekamer.nl/brief_in/20260915/aanbieding_mirt_overzicht_2027_en/f=/vn0zajvau3n8.pdf | yes | Disclaimer "Aan dit overzicht of de inhoud daarvan kunnen geen rechten worden ontleend."; "bijstukken bij de begroting 2027"; "kabinetsreactie … opgenomen". |

Stopped at 6 fetches. All core sentences confirmed; no further fetch needed.

## Writing

| # | Action | File | Decision |
|---|---|---|---|
| 33 | write | `NOTES/*.md` (21 files) | ≤40 lines each, no opinions. |
| 34 | write | `RWS-2.0.md` | Main blueprint. English rules, NL epigraphs with source ID. |
| 35 | write | `RWS-2.0-GLOSSARY.md` | NL mechanism → object name. |
| 36 | write | `RWS-2.0-MAPPING.md` | Manual section → Apparatus crate/type → v1/later. |
| 37 | write | `OPEN.md` | [NF], tensions, deliberate omissions. |
| 38 | write | `rws-2-0/SKILL.md`, `rws-2-0/references/RWS-2.0.md`, `rws-2-0/references/CHECKS.md` | Skill for Claude Code. |
| 39 | commit | — | NOT committed. Owner has not asked. Proposed message in chat. |
