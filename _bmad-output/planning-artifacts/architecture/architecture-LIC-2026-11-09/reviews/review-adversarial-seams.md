# Adversarial Seam Review

**Review target:** `ARCHITECTURE-SPINE.md` and companion `DATABASE-DESIGN.md`  
**Compared with:** approved stories in `_bmad-output/planning-artifacts/epics.md`  
**Verdict:** PASS — the previously reported cardinality mismatch is resolved; no other blocking seam mismatch found.

## Findings

### Resolved — CustomerPrincipal cardinality now matches

- **Before correction:** The architecture diagram modeled one-to-many while the database design modeled many-to-many.
- **Current architecture diagram:** `CUSTOMER }o--o{ CUSTOMERPRINCIPAL : maps` now models the same many-to-many relationship as the database design.
- **Database design:** the `CustomerPrincipal` section explicitly defines a many-to-many association, allowing one principal to act for one or more customer organizations; the database ERD also uses `CUSTOMER }o--o{ CUSTOMERPRINCIPAL`.
- **Result:** No longer divergent. User assignment still requires the principal's active mapping to include the licence's organization, keeping customer scope explicit.

## Checks that passed

- `LicencePlan` owns `LicenceType`, `MaxUsers`, and `MaxDevices`; architecture and database design both treat the caps independently.
- The architecture and database ERDs both require every issued licence to have one or more validity terms, user-assignment slots, and activation slots; the data design says issue creates the initial term and plan-defined slot counts.
- User assignments and device activations use separate slot types and histories; neither requires a user-device association.
- `LicenceTerm` stores immutable validity periods; the shared rule says a gap between terms is invalid and preserves old periods across renewal.
- Renewal is manual for active or expired time-limited licences; trials are non-renewable and perpetual licences do not require renewal.
- Licence policy is centralized in domain microflows; reporting is a read-only OData projection with a separately privileged internal role.
- Customer-scope enforcement derives from authenticated principal mapping, and external identifiers are not treated as authorization.
- The app baseline is described as the existing Mendix scaffold rather than a newly created application.

## Other seam checks

No other pair of independently implemented stories was found to imply incompatible entity ownership, lifecycle mutation, validity, caps, authorization, or reporting behavior. The architecture and database design agree on the approved plan fields, separate user/device slots, immutable validity terms, audit ownership, and read-only reporting contract.
