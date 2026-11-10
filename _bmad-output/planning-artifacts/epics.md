---
status: final
stepsCompleted:
  - step-01-validate-prerequisites
  - step-02-design-epics
  - step-03-create-stories
  - step-04-final-validation
inputDocuments:
  - "Conversation: LIC requirements analysis and decisions (2026-11-09)"
  - "_bmad-output/planning-artifacts/architecture/architecture-LIC-2026-11-09/ARCHITECTURE-SPINE.md"
  - "_bmad-output/planning-artifacts/architecture/architecture-LIC-2026-11-09/DATABASE-DESIGN.md"
---

# LIC - Delivery Plan

## Overview

This document captures requirements extracted for the LIC licence-management module before epic and story design. Source requirements came from the conversation and the current architecture/database design. There is no separate PRD or UX contract in the project yet.

## Requirements Inventory

### Functional Requirements

FR1: Internal administrators can manage products and customer organizations that receive licences.

FR2: An administrator can issue a licence to one customer organization for a product and licence plan.

FR3: The system supports time-limited, perpetual, and trial licences. Time-limited and trial licences have a start and end time; perpetual licences have no end time.

FR4: An administrator can configure independent user and device limits for a licence. Device activations do not need to be linked to assigned users.

FR5: The system creates a unique licence credential. It reveals the plaintext credential only at issuance and stores only a keyed digest.

FR6: A product client can validate a licence online and receive a valid/invalid result with a safe reason that does not reveal another customer's data or expose the licence credential.

FR7: The system records user and device assignments/activations against a licence and enforces its configured limits. Administrators can deactivate or release assignments according to the confirmed transfer policy.

FR8: An administrator can renew an eligible licence while preserving its history.

FR9: An administrator can suspend or revoke a licence. A suspended or revoked licence fails subsequent online validation.

FR10: The system records an audit event for issuance, renewal, suspension, revocation, validation outcomes as required by policy, and user/device assignment changes.

FR11: Internal administrators can view and manage all customer organizations. A customer user can only view data belonging to organizations mapped to that authenticated principal.

FR12: The system exposes approved reporting data to Power BI through a read-only, versioned Mendix OData v4 service. Power BI refreshes on a schedule and only the internal reporting audience can see system-wide data.

FR13: An administrator can search/filter licences by customer, product, plan, status, validity period, and licence identifier/prefix without retrieving the stored digest or plaintext key.

### NonFunctional Requirements

NFR1 — Security: Enforce role and customer-level authorization in Mendix entity access rules and microflows. Never authorize customer scope from a request parameter alone.

NFR2 — Secret protection: Do not persist plaintext licence keys, raw device fingerprints, credentials, or unnecessary personal data in audit logs or BI data.

NFR3 — Data integrity: Licence lifecycle transitions and their audit events are recorded consistently. User/device limits must not be exceeded under concurrent requests; hard-cap behavior is a release gate until proven on the selected Mendix Runtime.

NFR4 — Time correctness: Store timestamps consistently in UTC. Licence validity uses a half-open interval: valid from the start time up to, but not including, the end time.

NFR5 — Performance (provisional): The earlier analyst draft proposed online validation at no more than 500 ms at p95. This target has not been confirmed and is not a committed acceptance threshold.

NFR6 — Availability (provisional): The earlier analyst draft proposed 99.9% monthly availability. This target has not been confirmed; deployment, scale, and service objectives remain open.

NFR7 — Auditability: Important licence changes are traceable to actor, action, target, time, and correlation ID. Audit retention duration is not yet decided.

NFR8 — Compatibility: Keep the current Mendix project at version 11.11.0. PostgreSQL version must be verified against the selected Mendix runtime/hosting target.

NFR9 — Operability: Monitor validation/API failures and latency; define backup, restore, retention, RPO, and RTO before production.

### Additional Requirements

**Business requirements**

- BR1: Manage customer licences centrally by product and customer organization.
- BR2: Apply licence rights, limits, and validity periods consistently.
- BR3: Make licence state and safe failure reason visible to administrators and product clients.
- BR4: Allow licences to be renewed, suspended, or revoked with an auditable history.
- BR5: Support internal system-wide reporting while isolating customer-facing data.

**Architecture and platform constraints**

- Mendix 11.11.0 is the selected version and matches the current `LIC.mpr` model.
- Mendix Runtime owns the physical app schema in PostgreSQL; do not access or mutate generated app tables directly.
- Centralize licence rules in Mendix microflows/domain behavior so UI, validation service, and reporting do not implement divergent policy.
- Use a read-only OData v4 reporting contract with explicit View Entity access rules. Power BI must not query the Mendix-generated app schema directly.
- Resolve customer scope from the authenticated issuer/subject mapping. Select the identity provider during integration planning.
- Power BI is for the internal reporting audience and uses scheduled refresh.
- The stated environment is localhost/private infrastructure. Confirm whether localhost is development only and define the private production deployment topology before release.
- A hard user/device cap requires a concurrent-claim proof on the chosen Mendix Runtime. Do not rely on a unique token alone or direct SQL against runtime-owned tables.

**Open decisions to resolve during story design**

- **Decided:** User and device limits are separate, independent caps. Device activations are not required to be linked to a particular assigned user.
- How one device activation can be transferred/deactivated and whether licences support floating/concurrent user semantics.
- Exact online validation request identity, product-client authentication, response reason vocabulary, and rate limiting.
- Default trial duration and grace-period policy remain open; in v1 the administrator enters the trial validity dates. Renewal is manual for active or expired Time-limited licences only; Trial licences are not renewable and Perpetual licences do not need renewal.
- Power BI refresh principal/authentication and exact scheduled refresh frequency.
- Identity provider, private hosting topology, supported PostgreSQL patch, expected volume, availability, backup, RPO/RTO, and retention targets.
- Confirm or replace provisional NFR targets: validation latency p95 500 ms and availability 99.9% monthly.

### UX Design Requirements

No UX design contract (`DESIGN.md` + `EXPERIENCE.md`) was found or supplied. Page layouts, interaction patterns, accessibility details, and responsive behavior are not yet specified.

### FR Coverage Map

FR1: Epic 1 — Manage customer organizations and products.
FR2: Epic 1 — Issue an organization licence for a product and plan.
FR3: Epic 1 — Support time-limited, perpetual, and trial licences.
FR4: Epic 1 — Configure user and device limits.
FR5: Epic 1 — Protect licence credentials and reveal them only at issuance.
FR6: Epic 1 — Validate licences online and return safe outcomes.
FR7: Epic 1 — Assign/release users and devices while enforcing limits.
FR8: Epic 1 — Renew eligible licences and preserve history.
FR9: Epic 1 — Suspend and revoke licences.
FR10: Epic 1 — Audit important licence changes.
FR11: Epic 1 — Enforce internal-admin and customer-organization data scope.
FR12: Epic 2 — Publish internal licence reporting through scheduled Power BI refresh.
FR13: Epic 1 — Search/filter licences using safe identifiers and lifecycle fields.

## Workstream Overview

### Epic 1: Cấp và kiểm soát licence cho tổ chức

Internal administrators can manage customer organizations, products, plans, and complete licence lifecycles. Product clients can validate licences online, while users and devices are assigned within both configured limits. Customer users only see data for their authorized organization, and important changes are auditable.

**FRs covered:** FR1–FR11, FR13.

**Implementation notes:** Keep policy in Mendix domain microflows; implement licence secret protection, online validation, renew/suspend/revoke, customer scope, audit, and concurrent assignment limits. A hard user/device cap cannot be released until concurrent claim behavior is proven on the selected Mendix Runtime.

### Epic 2: Theo dõi danh mục licence bằng Power BI

Internal reporting users can review licence status, expiry, assignment/activation usage, and activity through Power BI with scheduled refresh, without direct access to Mendix-generated PostgreSQL tables.

**FRs covered:** FR12.

**Implementation notes:** Expose a versioned, read-only Mendix OData v4 service over reporting View Entities. Define View Entity access rules explicitly and restrict system-wide data to the internal reporting role. This epic depends on the operational licence data delivered by Epic 1.

## Epic 1: Cấp và kiểm soát licence cho tổ chức

Internal administrators can manage customer organizations, products, plans, and complete licence lifecycles. Product clients can validate licences online, while users and devices are assigned within both configured limits. Customer users only see data for their authorized organization, and important changes are auditable.

### Story 1.1: Quản lý tổ chức khách hàng

As an internal administrator,
I want to create and archive customer organization records,
So that licences are issued to the correct organization and its history is preserved.

**FRs:** FR1.

**Acceptance Criteria:**

**Given** an internal administrator with organization-management permission  
**When** they create an organization with a unique code and valid name  
**Then** the system creates it in Active state and records its creation time.

**Given** the organization code already exists or required data is missing  
**When** the administrator saves the organization  
**Then** the system rejects the save and identifies the validation error.

**Given** an organization has licences or audit history  
**When** an administrator removes it from active use  
**Then** the system archives it instead of hard-deleting it and preserves its history.

**Given** an organization is archived  
**When** an administrator views its existing licences  
**Then** the licence history remains available and existing licences do not change state solely because the organization was archived.

### Story 1.2: Thiết lập sản phẩm và licence plan

As an internal administrator,
I want to define products and versioned licence plans with licence types and user/device limits,
So that licences are issued under consistent, reusable terms.

**FRs:** FR1, FR3, FR4.

**Acceptance Criteria:**

**Given** an administrator with product-management permission  
**When** they create a product with a unique code and valid name  
**Then** the product is saved in Active state.

**Given** an administrator creates a plan for a product  
**When** they select `Time-limited`, `Perpetual`, or `Trial`, set positive user and device limits, and provide a valid plan code/version  
**Then** the plan is saved with the selected product, type, limits, and version.

**Given** either the user limit or device limit is zero or negative  
**When** the administrator saves the plan  
**Then** the system rejects the plan and identifies the invalid limit.

**Given** a product already has the same plan code and version  
**When** the administrator attempts to create that combination again  
**Then** the system rejects the duplicate.

**Given** a plan version has been used to issue a licence  
**When** an administrator needs to change its terms  
**Then** the system preserves the existing version and requires a new version for the changed terms.

### Story 1.3: Cấp licence cho tổ chức

As an internal administrator,
I want to issue a licence to an organization using an approved product plan,
So that the organization can use the product under the granted terms.

**FRs:** FR2, FR3, FR5.

**Acceptance Criteria:**

**Given** the organization, product, and plan are valid  
**When** an administrator issues a licence with a valid start time  
**Then** the system creates a licence linked to the selected records and records the issuer and issue time.

**Given** the plan is Time-limited or Trial  
**When** the administrator issues the licence  
**Then** an end time after the start time is required.

**Given** the plan is Perpetual  
**When** the administrator issues the licence  
**Then** the licence has no expiry time.

**Given** a licence is issued successfully  
**When** the system returns the result  
**Then** plaintext key material is shown only in that issuance response and only a keyed digest and non-sensitive prefix are stored.

**Given** an administrator later views an issued licence  
**When** they open its details  
**Then** the system does not reveal the plaintext key again.

### Story 1.4: Gán user vào licence

As an internal administrator,
I want to assign or remove customer users from a licence,
So that I can enforce the user limit and keep access within the correct customer organization.

**FRs:** FR4, FR7, FR11.

**Acceptance Criteria:**

**Given** a licence belongs to organization A and a user principal is mapped to organization A  
**When** an administrator assigns that user  
**Then** the assignment is saved and counts toward the active user limit.

**Given** a user is already assigned to the licence  
**When** the administrator submits the same assignment again  
**Then** the system does not create a duplicate assignment or increase the active-user count.

**Given** the active-user count has reached `MaxUsers`  
**When** an administrator attempts to assign another user  
**Then** the system rejects the assignment and does not exceed the limit.

**Given** a user principal is mapped to a different organization  
**When** an administrator attempts to assign that user to the licence  
**Then** the system rejects the assignment.

**Given** an administrator removes a user from a licence  
**When** the removal succeeds  
**Then** the user slot is released and assignment history is retained.

**Given** two concurrent assignment requests contend for the final user slot  
**When** both requests are processed  
**Then** active assignments do not exceed `MaxUsers`; this concurrency behavior must be verified on the selected Mendix Runtime before release.

### Story 1.5: Kích hoạt và gỡ thiết bị

As a product client,
I want to activate or deactivate a device on a licence,
So that authorized devices can use the product without exceeding the device limit.

**FRs:** FR4, FR7.

**Acceptance Criteria:**

**Given** a licence is valid and a device is not active on it  
**When** an authorized product client requests activation  
**Then** the system creates an activation and stores only a keyed device-fingerprint digest.

**Given** the same device is already active on the licence  
**When** the client retries the activation request  
**Then** the system does not create a duplicate activation or consume another slot.

**Given** the active-device count has reached `MaxDevices`  
**When** a client requests activation for another device  
**Then** the system rejects the request with a safe reason.

**Given** two concurrent requests contend for the final device slot  
**When** both requests are processed  
**Then** active devices do not exceed `MaxDevices`; this behavior must be verified on the selected Mendix Runtime before release.

**Given** a device is deactivated  
**When** the deactivation succeeds  
**Then** the slot is released and activation history is retained.

**Given** user and device limits are counted independently  
**When** a device is activated  
**Then** the operation does not create or change a user assignment.

### Story 1.6: Xác thực licence online

As a product client,
I want to validate a licence online for the current product, user, and device,
So that the product can allow only authorized use and explain safe validation failures.

**FRs:** FR6, FR9, FR10.

**Acceptance Criteria:**

**Given** the key identifies an active licence for the requested product, the user is assigned, the device is active, and the licence is within its validity period
**When** the client submits an online validation request
**Then** the system returns a valid result.

**Given** the licence is not yet valid, expired, suspended, or revoked
**When** the client submits a validation request
**Then** the system returns an invalid result with a safe reason code.

**Given** the key is unknown or does not match the requested product
**When** the client submits a validation request
**Then** the system returns an invalid result without revealing whether the key belongs to another organization.

**Given** the user is not assigned or the device is not active on the licence
**When** the client submits a validation request
**Then** the system returns an invalid result with a safe reason code and does not authorize product use.

**Given** a validation request is missing required context or contains invalid input
**When** the request is processed
**Then** the system rejects it without authorizing use.

**Given** a validation request succeeds or fails
**When** the system records operational or audit data
**Then** it does not store the plaintext key or raw device fingerprint in logs.

### Story 1.7: Gia hạn licence

As an internal administrator,
I want to manually renew an active or expired time-limited licence,
So that an organization can continue using the product under a new validity period while the prior period remains traceable.

**FRs:** FR8, FR10.

**Acceptance Criteria:**

**Given** an administrator has licence-management permission and selects a time-limited licence that is active or expired
**When** they enter the start and end of a new validity period, with the end after the start
**Then** the system records the renewal and its new validity period without overwriting the prior period's history.

**Given** a licence is Trial
**When** an administrator views its lifecycle actions
**Then** the system does not offer renewal for that licence.

**Given** the renewal is saved
**When** the system records the change
**Then** it records the acting administrator, renewal time, and new validity period.

**Given** the proposed renewal period is invalid
**When** the administrator submits it
**Then** the system rejects the renewal and leaves the existing licence history unchanged.

**Given** a licence is Perpetual
**When** an administrator views its lifecycle actions
**Then** the system does not require or offer a renewal action for that licence.

### Story 1.8: Tạm ngưng và thu hồi licence

As an internal administrator,
I want to suspend or revoke a licence with a recorded reason,
So that I can stop unauthorized or no-longer-permitted use and retain an auditable lifecycle history.

**FRs:** FR9, FR10.

**Acceptance Criteria:**

**Given** an administrator has licence-management permission and selects an active licence
**When** they suspend it with a reason
**Then** the system changes its status to Suspended and records the actor, time, and reason.

**Given** an administrator has licence-management permission and selects an active licence
**When** they revoke it with a reason
**Then** the system changes its status to Revoked and records the actor, time, and reason.

**Given** a licence is Suspended or Revoked
**When** a product client validates it online
**Then** validation returns invalid with a safe reason and does not authorize use.

**Given** a licence has lifecycle history
**When** it is suspended or revoked
**Then** prior issuance, renewal, assignment, and activation history remains available and is not hard-deleted.

### Story 1.9: Phân quyền dữ liệu theo tổ chức

As an internal administrator or customer user,
I want access to be scoped to my authorized role and organization,
So that internal administrators can manage the full portfolio while customers can only view their own organization's data.

**FRs:** FR11.

**Acceptance Criteria:**

**Given** an authenticated internal administrator with the appropriate role
**When** they view or manage customer data
**Then** they can access all organizations allowed by their internal permissions.

**Given** an authenticated customer principal is mapped to organization A
**When** they view customer-facing data
**Then** the system returns only data belonging to organization A.

**Given** a customer principal mapped to organization A requests data for organization B by changing a request parameter or identifier
**When** the system authorizes the request
**Then** it denies access and does not disclose organization B's data.

**Given** a customer principal has no valid organization mapping
**When** they request customer data
**Then** the system denies access by default.

### Story 1.10: Tra cứu licence

As an internal administrator,
I want to search and filter licences by organization, product, plan, status, validity period, and safe identifier,
So that I can find and manage the correct licence efficiently.

**FRs:** FR13.

**Acceptance Criteria:**

**Given** an internal administrator has licence-view permission
**When** they filter licences by organization, product, plan, status, or validity period
**Then** the result contains only licences matching all supplied filters.

**Given** an administrator searches by a licence identifier or safe key prefix
**When** matching licences are returned
**Then** the system returns only the approved non-secret identifier fields.

**Given** a search or filter result is displayed
**When** the administrator views the result or licence summary
**Then** neither plaintext key material nor the stored keyed digest is exposed.

**Given** a user without internal licence-view permission requests the internal search
**When** the request is authorized
**Then** the system denies access.

### Story 1.11: Nhật ký kiểm toán licence

As an internal administrator with audit-view permission,
I want important licence operations and outcomes recorded in an audit trail,
So that changes and validation decisions can be traced and investigated.

**FRs:** FR10.

**Acceptance Criteria:**

**Given** an administrator issues, renews, suspends, or revokes a licence
**When** the operation is processed
**Then** the audit trail records the actor, action, target, timestamp, correlation ID, and outcome.

**Given** a user assignment or device activation changes
**When** the change is processed
**Then** the audit trail records the change with its actor, target, timestamp, correlation ID, and outcome.

**Given** an online validation request is processed
**When** its outcome is recorded under the audit policy
**Then** the event records the outcome and safe reason without recording plaintext key material or raw device fingerprint.

**Given** an audit event is recorded
**When** normal application users access the system
**Then** only authorized internal users can read the audit history and normal application functions cannot alter or delete it.

**Given** audit data is written
**When** the system persists event details
**Then** it excludes plaintext licence keys, raw device fingerprints, and unnecessary sensitive personal data.

## Epic 2: Theo dõi danh mục licence bằng Power BI

Internal reporting users can review licence status, expiry, assignment/activation usage, and activity through Power BI with scheduled refresh, without direct access to Mendix-generated PostgreSQL tables.

### Story 2.1: Cung cấp dữ liệu licence cho báo cáo

As an internal reporting user,
I want a versioned, read-only OData contract containing approved licence reporting data,
So that Power BI can report on the licence portfolio without querying the Mendix application schema directly.

**FRs:** FR12.

**Acceptance Criteria:**

**Given** the reporting service is published
**When** the internal reporting role requests the supported OData v4 version
**Then** it can read approved licence summary, validity/status, user/device limit and utilization, and activity data needed by the reports.

**Given** a user without the internal reporting role requests the reporting service
**When** Mendix authorizes the request
**Then** access to system-wide reporting data is denied.

**Given** a reporting entity is exposed through OData
**When** a client reads or queries it
**Then** the service is read-only and does not permit creating, updating, or deleting application data.

**Given** data is exposed for reporting
**When** the service returns records and attributes
**Then** it excludes plaintext licence keys, stored key digests, raw device fingerprints, and unnecessary personal data.

**Given** a new reporting contract version is introduced
**When** the service is deployed
**Then** the existing version remains available or a documented migration path is provided so report consumers can move without an undocumented breaking change.

### Story 2.2: Kết nối Power BI và refresh định kỳ

As an internal reporting user,
I want Power BI to consume the approved OData service and refresh reports on a schedule,
So that internal stakeholders can review current system-wide licence information without direct database access.

**FRs:** FR12.

**Acceptance Criteria:**

**Given** Power BI is configured to consume the approved OData v4 service
**When** it authenticates using the internal reporting principal
**Then** it can read the approved reporting data without receiving write access to the application.

**Given** a user is outside the authorized internal reporting audience
**When** they attempt to view system-wide licence reports
**Then** access is denied.

**Given** a refresh schedule is configured
**When** the scheduled refresh runs
**Then** Power BI refreshes from the OData service and makes the latest successfully refreshed dataset available to authorized users.

**Given** a scheduled refresh fails
**When** the failure is reported by the integration
**Then** the operational status exposes the failure for investigation without exposing credentials or sensitive licence secrets.

**Given** integration details are finalized
**When** the Power BI connection is configured
**Then** the reporting principal/authentication method and refresh frequency are recorded as deployment configuration.
