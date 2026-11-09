# Licence Management — Data Design for Mendix + PostgreSQL

**Status:** Draft for review  
**Companion to:** [ARCHITECTURE-SPINE.md](ARCHITECTURE-SPINE.md)  
**Persistence:** Mendix domain model persisted by Mendix Runtime in PostgreSQL  
**Date:** 2026-11-09

## 1. Persistence boundary

Mendix Runtime owns the physical PostgreSQL schema for persistent domain entities. Define entities, attributes, associations, validation, and access rules in Mendix Studio Pro. Do not create, rename, or update generated tables through direct SQL or treat them as a stable external integration contract.

For Mendix Cloud, Mendix provisions PostgreSQL persistence for the app. For private cloud or self-hosted deployment, confirm the exact PostgreSQL major/minor version against the selected Mendix Runtime and infrastructure. Each Mendix app must have its own database; cross-app/reporting access uses supported APIs or an explicitly separate reporting store.

This document is the **logical data model**, not a hand-authored DDL specification. Mendix internal object identifiers are runtime-managed and are not public API identifiers. Add opaque external IDs where consumers need stable references.

## 2. Logical entity relationship model

```mermaid
erDiagram
    CUSTOMER ||--o{ LICENCE : receives
    CUSTOMER }o--o{ CUSTOMERPRINCIPAL : authorizes
    PRODUCT ||--o{ LICENCEPLAN : offers
    LICENCEPLAN ||--o{ PLANENTITLEMENT : contains
    LICENCEPLAN ||--o{ LICENCE : grants
    LICENCE ||--o{ ACTIVATION : has
    LICENCE ||--o{ AUDITEVENT : records
    CUSTOMER ||--o{ AUDITEVENT : scopes
    LICENCE ||--o{ ACTIVATIONSLOT : limits
    ACTIVATIONSLOT ||--o{ ACTIVATION : history
```

## 3. Domain entities

Recommended Mendix module: `LicenceDomain`. Mark records with lifecycle history as non-deletable through page, microflow, and entity access rules. Attribute names are conceptual; use Mendix types that map cleanly to PostgreSQL and published OData.

### `Customer`

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `ExternalId` | String (UUID text) | Required, unique, immutable after creation |
| `CustomerCode` | String | Required, unique |
| `Name` | String | Required |
| `ContactEmail` | String | Optional PII; do not publish to BI unless explicitly required |
| `LifecycleState` | Enumeration | `Active`, `Archived` |
| `CreatedAt`, `UpdatedAt` | DateTime | Required, UTC |

### `CustomerPrincipal`

Maps an authenticated external identity to one or more customer organizations. Resolve this mapping from the authenticated principal; never authorize a request based only on a client-supplied customer ID.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `Issuer` | String | Required; normalized identity-provider issuer identifier |
| `ExternalPrincipalId` | String | Required; stable subject ID from that issuer, not email |
| `PrincipalKey` | String | Required, unique; deterministic canonical combination of `Issuer` and `ExternalPrincipalId` |
| `LifecycleState` | Enumeration | `Active`, `Disabled` |

Association: many-to-many between `CustomerPrincipal` and `Customer`, allowing a principal to act for one or more organizations. Platform administrators use a separate privileged module role. Resolve identity by the `(Issuer, ExternalPrincipalId)` pair so switching or adding an identity provider cannot collide subjects.

### `Product`

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `ExternalId` | String (UUID text) | Required, unique, immutable |
| `ProductCode` | String | Required, unique |
| `Name` | String | Required |
| `LifecycleState` | Enumeration | `Active`, `Archived` |
| `CreatedAt`, `UpdatedAt` | DateTime | Required, UTC |

### `LicencePlan`

An immutable version of a product's licensing terms. Create a new version when the plan changes; do not silently change terms for already-issued licences.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `ExternalId` | String (UUID text) | Required, unique, immutable |
| `PlanCode` | String | Required |
| `Version` | Integer | Required, > 0 |
| `MaxActivations` | Integer | Required, > 0 |
| `LifecycleState` | Enumeration | `Draft`, `Active`, `Retired` |
| `CreatedAt`, `CreatedBy` | DateTime, String | Required |

Association: `LicencePlan_Product` is many plans to one product. Enforce a unique `(Product, PlanCode, Version)` combination in domain logic and prevent edits to terms once the plan has issued licences. Mendix entity-level uniqueness may not cover a composite key; if concurrent plan creation must be guaranteed, validate a dedicated unique `PlanVersionKey` string.

### `PlanEntitlement`

One entitlement for one plan. Keep a stable entitlement code and a typed value (for example string, integer, or boolean) rather than unvalidated JSON where possible.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `EntitlementCode` | String | Required |
| `ValueText` | String | Optional, type-specific interpretation |
| `ValueInteger` | Integer | Optional |
| `ValueBoolean` | Boolean | Optional |

Association: many `PlanEntitlement` records to one `LicencePlan`. Domain validation ensures exactly one value field is set as required by the entitlement definition. Keep secrets out of entitlement values.

### `Licence`

One customer grant for one product plan. Do not persist a mutable `Expired` state; derive effective state from lifecycle state and validity timestamps.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `ExternalId` | String (UUID text) | Required, unique, immutable; public-safe reference |
| `KeyDigest` | String | Required, unique; keyed HMAC digest, never raw key |
| `KeyPrefix` | String | Required; non-secret lookup hint |
| `KeyVersion` | Integer | Required; digest/key-generation version |
| `LifecycleState` | Enumeration | `Active`, `Suspended`, `Revoked` |
| `ValidFrom` | DateTime | Required, UTC |
| `ValidUntil` | DateTime | Optional; exclusive upper bound; empty means perpetual |
| `IssuedAt`, `IssuedBy` | DateTime, String | Required |
| `RevokedAt`, `RevokedBy`, `RevocationReason` | DateTime, String, String | Required only when revoked; reason should not contain sensitive details |
| `CreatedAt`, `UpdatedAt` | DateTime | Required |

Associations: many `Licence` records to one `Customer`; many `Licence` records to one immutable `LicencePlan`. Use domain validation so `ValidUntil > ValidFrom`, and revocation fields match `LifecycleState`.

Effective state precedence: `Revoked`, then `Suspended`, then `Pending` if now is before `ValidFrom`, then `Expired` if now is at or after `ValidUntil`, otherwise `Active`. Retiring a plan or archiving a customer does not silently change an issued licence in this initial model; define explicit suspension/revocation actions if business policy requires it.

Generate licence secrets with a cryptographically secure random source and return the plaintext only once. Store only an HMAC digest in `KeyDigest`; keep the pepper in deployment secret storage, not in source control or ordinary Mendix constants. Rotation may require re-issuing keys because raw values are not recoverable. Never publish the digest or raw key through OData.

### `Activation`

One historical device activation/deactivation record. Store a keyed digest of the device fingerprint, not raw hardware identifiers.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `ExternalId` | String (UUID text) | Required, unique |
| `DeviceDigest` | String | Required; keyed digest |
| `DeviceLabel` | String | Optional; user-provided and potentially identifying |
| `ActivatedAt` | DateTime | Required |
| `DeactivatedAt` | DateTime | Empty means active |
| `LastValidatedAt` | DateTime | Optional |

Associations: many `Activation` records to one `Licence`, and many historical `Activation` records to one `ActivationSlot`. Exactly zero or one activation associated with a slot may have `DeactivatedAt` empty. Prevent duplicate active `(Licence, DeviceDigest)` claims in the activation microflow. Use `ActivationSlot` entities to represent a finite number of seats; concurrent claims against the same slot must be tested on the chosen Mendix Runtime. If optimistic conflict/retry cannot guarantee the limit, use a separately owned activation service/store rather than unsupported direct SQL against Mendix tables.

### `ActivationSlot`

Optional modeled seat allocation when `MaxActivations` is a hard cap. Create the allowed number of slot entities for the issued licence; a slot has either no active activation or one active activation.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `SlotNumber` | Integer | Required; unique within the licence, enforced by a composite token |
| `SlotToken` | String | Required, unique; generated from licence external ID plus slot number. Identifies a slot but does not by itself guarantee atomic occupancy. |

Association: many `ActivationSlot` entities to one `Licence`; each `Activation` references one slot. A slot is reused only after deactivation, preserving activation history. Enforce single active occupancy in the activation microflow and prove atomic conflict behavior before enabling a hard cap.

### `AuditEvent`

Append-only history for significant business and security actions.

| Attribute | Mendix type | Rules |
| --- | --- | --- |
| `ExternalId` | String (UUID text) | Required, unique |
| `ActorType` | Enumeration | `Admin`, `Client`, `System` |
| `ActorId` | String | Optional external principal reference |
| `Action` | String or Enumeration | Required controlled action vocabulary |
| `TargetType`, `TargetExternalId` | String | Required |
| `OccurredAt` | DateTime | Required, UTC |
| `CorrelationId` | String | Optional UUID text |
| `SafeMetadata` | String/structured child entity | Allowlisted non-sensitive details only |

Associations: optional many events to one `Customer`, optional many events to one `Licence`. Restrict updates/deletes through module security; create events through the same microflow that performs the business command. Do not put keys, digests, credentials, contact details, or raw device identifiers in audit metadata.

## 4. Mendix security and operations

- Define app, module, and entity access rules for administrator, product-client, and reporting roles.
- Keep destructive delete unavailable for issued licences, plans with issued grants, activations, and audit events; use archive/revocation transitions.
- Expose only explicit operations through published services. Do not enable CRUD wholesale on licence or audit entities.
- Use microflows for issue, renew, suspend, revoke, validate, activate, and deactivate. Each flow validates authorization and business invariants before committing.
- Add an idempotency token to service operations that may be retried, and persist the request outcome so retries do not issue duplicate licences or activations.
- Monitor Mendix Runtime errors and service latency. Confirm database backup, restore, retention, RPO, and RTO with the selected deployment target.

## 5. Power BI integration

Publish a versioned, **read-only OData v4** service from a dedicated `LicenceReporting` module using Mendix View Entities over the domain model. View Entities are query-backed and read-only; enable OQL v2 and verify query performance with expected refresh volume. Mendix documents View Entities as publishable through OData and OData as a supported BI route. Configure app security, allowed module roles, and access rules directly on each View Entity: do not assume the source entities' access rules automatically protect a View Entity. Restrict the default Power BI principal to the internal reporting role; expose customer-scoped reporting only after proving row-level customer filtering works for that identity model.

Initial read-only View Entities:

- `LicenceSummary`: licence external ID, customer code/name, product/plan/version, issue date, validity dates, derived state, activation count, revocation timestamp.
- `LicenceActivityDaily`: date, product, plan, action category, aggregated event count.
- `ActivationUtilization`: licence external ID, activation limit, active activation count, utilization ratio.

Exclude `KeyDigest`, `KeyPrefix` where not needed, contact email, actor identity, device digest, raw audit metadata, and internal Mendix IDs. Start with Power BI Import and scheduled refresh; only enable write operations if there is a separate, explicitly reviewed use case. If analytics requires direct PostgreSQL/DirectQuery at higher volume, create a separately owned reporting database or warehouse populated through supported Mendix APIs/exports. Never point Power BI at the Mendix-generated app schema.

## 6. Logical integrity and indexes

Use Mendix entity uniqueness where available for `ExternalId`, product/customer codes, key digest, and slot token. For composite uniqueness such as plan version or active device per licence, use a dedicated unique token attribute and validate behavior under concurrent requests. Keep entity associations restrictive; archive rather than cascade-delete business history.

The physical indexes and foreign keys are generated/managed by Mendix Runtime. Do not add manual indexes directly to a Mendix-managed database unless the selected Mendix hosting/runtime explicitly supports and documents that operation. Review slow OData and microflow queries using Mendix-supported monitoring and optimization tools.

## 7. Open decisions before production

- Confirm customer/user/device ownership semantics, seat types, activation transfer, and offline validation requirements.
- Confirm Mendix Cloud, private cloud, or self-hosted deployment. The current Mendix docs list PostgreSQL 14–18 as supported, but exact minor/runtime compatibility must be verified for the selected environment.
- Prototype concurrent activation-slot claims on the chosen Mendix Runtime; this is a key integrity gate for a hard device cap.
- Confirm identity provider, published-service authentication, refresh credentials for Power BI, and tenant-level row visibility.
- Confirm data retention, audit retention, deletion/anonymization rules, availability, backup, RPO/RTO, and expected volume/refresh cadence.

## 8. References

- [Mendix data storage](https://docs.mendix.com/refguide/data-storage/)
- [Mendix system requirements](https://docs.mendix.com/refguide/system-requirements/)
- [Mendix OData services](https://docs.mendix.com/refguide/published-odata-services/)
- [Mendix: expose data to BI tools using OData](https://docs.mendix.com/refguide/exposing-data-to-bi-tools-using-odata/)
- [Mendix View Entities](https://docs.mendix.com/refguide/view-entities/)
- [Mendix: decoupling APIs with View Entities](https://docs.mendix.com/refguide/decoupling-apis/)
- [Microsoft Power Query OData Feed connector](https://learn.microsoft.com/power-bi/desktop-connect-odata)
