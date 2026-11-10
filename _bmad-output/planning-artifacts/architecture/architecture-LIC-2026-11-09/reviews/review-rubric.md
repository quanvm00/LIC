# Architecture Rubric Review — LIC

**Review target:** `../ARCHITECTURE-SPINE.md` and companion `../DATABASE-DESIGN.md`  
**Review date:** 2026-11-09  
**Verdict:** PASS, with explicit release and production follow-ups

## Scope and evidence

- Reviewed the architecture spine, logical database design, and approved LIC epics/stories.
- Checked the recorded mxcli baseline: `APP/LIC.mpr`, Mendix 11.11.0, nine modules; `MyFirstModule` contains `Home_Web` and `MyFirstLogic` and no domain entities.
- Confirmed the documents no longer claim that no Mendix source exists or that user/device entitlement semantics remain undecided.

## Good-spine checklist

| Check | Result | Notes |
| --- | --- | --- |
| Fixes divergence points for downstream stories | Pass | Shared microflow policy, PostgreSQL ownership, immutable terms, independent user/device caps, customer authorization, and read-only reporting are explicit. |
| Every AD has Binds, Prevents, and Rule | Pass | AD-1 through AD-8 state all three. |
| Deferred decisions cannot silently split implementations | Pass with follow-ups | Identity provider, service authentication, deployment topology, retention, SLOs, and transfer semantics are explicitly deferred/open. |
| Technology is verified and fits | Pass | Mendix 11.11.0 matches the app. PostgreSQL exact deployment version remains open and is called out for compatibility verification. |
| Ratifies the brownfield application | Pass | Documents identify the existing scaffold and protect Marketplace/System modules from feature changes. |
| Covers approved capability requirements | Pass | Licence types, organization ownership, issue/renewal/revoke, user assignment, device activation, validation, audit, tenant scope, and BI reporting are represented. |
| All owned architecture dimensions are decided, deferred, or open | Pass | Application boundary and persistence are decided; identity, hosting and operations are explicit open items. |
| Architecture and database design are consistent | Pass | Plan type and separate limits, immutable term history, user assignment history, independent device activation, and slot claims appear consistently in both. |

## Findings and follow-ups

### Medium — Prove both concurrent hard caps before release

`MaxUsers` and `MaxDevices` are distinct requirements. The data design models separate `UserAssignmentSlot` and `ActivationSlot` claims, and AD-5 requires atomic claims and conflict/retry proof. Keep that runtime proof as a release gate; a unique key by itself is not sufficient.

### Medium — Resolve integration and production settings before deployment

Identity provider and product-client authentication, Power BI refresh credentials, whether localhost is development-only, exact private topology/PostgreSQL version, retention, backups, RPO/RTO, and availability targets remain open. The spine correctly marks these for integration or production rather than guessing.

### Low — Confirm Mendix handling for active-key uniqueness

The logical design uses `ActiveAssignmentKey` and `ActiveDeviceKey` to prevent duplicate active claims, clearing them when a claim is released. During implementation, confirm the selected Mendix model/runtime can enforce this uniqueness pattern with the intended empty/null behavior; otherwise use an equivalent modeled constraint and retain the concurrency proof.

## Recommendation

The spine and companion database design are coherent enough to proceed to Sprint Planning. Preserve the hard-cap and deployment checks as explicit release/production gates.
