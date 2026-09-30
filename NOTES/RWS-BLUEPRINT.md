# NOTES — Aparatus/RWS-Blueprint Foundation Architecture.md (1.0)

## What it regulates
- Three layers: Owner (charter) / Core (log, registries, validation) / Service Providers (surfaces).
- Boundary rules: providers speak only envelope events to Core API; Core holds no module logic; charter read by code, written by Owner only.
- Canon invariants: append-only; full envelope; raw canonical, projections disposable; provenance mandatory; gaps are data; failures are events; sensitive scope needs charter amendment.
- Module permit: registry entry, namespace, schemas, admission = Owner decision recorded as event.
- Maintenance windows, restore test monthly.

## Not liftable (per brief)
- iOS/widget/Apple free tier, VRCM-AMG tenant, FastAPI pick, homelab specifics, brand metaphors.

## Objects (liftable)
- module admission event, gap event, failure event kinds, deprecated flag instead of delete.
