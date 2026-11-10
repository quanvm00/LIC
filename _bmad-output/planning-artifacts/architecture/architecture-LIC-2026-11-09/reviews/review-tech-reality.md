# Technology and Brownfield Reality Review

## Verdict

**PASS with deferred operational decisions.** The updated architecture correctly describes the existing Mendix app scaffold, preserves the selected Mendix version, and does not claim an unverified PostgreSQL deployment target.

## Checks

- **mxcli baseline:** Consistent with the observed model: `APP/LIC.mpr` is Mendix 11.11.0 and mxcli reported nine modules. `MyFirstModule` has `Home_Web` and `MyFirstLogic` with no domain entities; other listed modules are Marketplace modules or `System`. The architecture no longer calls the repository greenfield and says to extend the existing MPR.
- **Mendix version:** The architecture pins 11.11.0, matching the MPR and the user’s decision. Official Mendix 11.11 release notes identify 11.11.0 and its release date.
- **PostgreSQL compatibility:** Official Mendix system requirements list PostgreSQL 14, 15, 16, 17, and 18. The architecture and database design leave the exact deployed version to be verified against the selected private Runtime/host, which is appropriate because the production topology is not settled.
- **Data model alignment:** The companion logical design now includes `LicenceType`, separate `MaxUsers`/`MaxDevices`, `LicenceTerm`, `UserAssignment`, and separate user/device slot entities. Device activation is not associated with a user. These match the approved story decisions.
- **Integration:** OData v4/View Entities and Power BI scheduled Import remain the design; no direct Power BI access to Mendix-managed PostgreSQL tables is introduced.

## Deferred items / release gates

- Confirm whether localhost is development-only and select private production topology and exact PostgreSQL version before deployment.
- Select identity-provider and published-service authentication details during integration.
- Prove both user-assignment and device-activation slot claims under concurrent requests on the selected Mendix Runtime before release. The architecture correctly states this as a hard release gate and provides a separately owned atomic service/store fallback.

## Sources checked

- mxcli output for `show -p APP/LIC.mpr modules` and `structure -p APP/LIC.mpr -d 2 -m MyFirstModule` (2026-11-09).
- [Mendix Studio Pro 11.11 release notes](https://docs.mendix.com/releasenotes/studio-pro/11.11/).
- [Mendix system requirements](https://docs.mendix.com/refguide/system-requirements/).
