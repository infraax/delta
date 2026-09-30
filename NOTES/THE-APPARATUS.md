# NOTES — Aparatus/The Apparatus.md + Design.md (1.0 architecture, v0.1)

## What it regulates
- Design laws: local ownership; one law for exchanges; many project stores; raw immutable; identity/time/provenance/authority on every object; policy/coordination/execution/evidence separate; no automatic authority escalation; failure first-class; safe degradation; dependencies are assets.
- Authority table: Owner / Core / Project Steward / Service Provider / Worker / Independent Evaluator.
- Objects: Artifact, RecordReceipt, Claim, Evidence, Decision, Task, TaskLease, SessionBundle, ProtocolRun, CapabilityGrant.
- Protocols: project.bootstrap, artifact.intake, session.close, task.lease, high_impact.action, maintenance.nightly.
- ADR-0001: Rust, SQLite, custom CAS + ledger + leases; Cedar behind PolicyEngine trait; RequireApproval is protocol state.

## Implemented in repo (M0)
- apparatus-types: ObjectId (UUIDv7), ProjectId, ArtifactId, ReceiptId, PrincipalId, CorrelationId; Classification {Public, Restricted, Internal}; Provenance {source, creator}; ObjectHeader {id, created_at, project_id, classification, provenance, correlation_id}.
- apparatus-ledger: Receipt {header, previous_receipt_id, previous_hash, operation_payload: JSON}; compute_hash (non-canonical JSON).
- apparatus-schema: Validate; Restricted requires provenance; created_at ≠ 0.
- apparatus-store: traits ArtifactStore, ReceiptLedger, Transaction. No impl.
- apparatus-artifacts: CAS path sha256/ab/cd/<hash>.
- CLI: `doctor` only.

## Gaps
- No policy crate, no registry crate, no protocol runner in code. Cedar deferred.
