# MT-0002 – Technical Design Specification

**Document ID:** MT-0002  
**Document Title:** Technical Design Specification  
**Project:** 3C Migration Toolkit (MT)  
**Version:** 0.1  
**Status:** Draft  
**Phase:** Phase 0 – Project Foundation  
**Author:** Bobby Rufener / 3C Care Systems  
**Created:** 2026-06-29  
**Last Updated:** 2026-06-29  

---

# 1. Purpose

This document defines the engineering standards, implementation patterns, interfaces, execution lifecycle, diagnostics, and coding conventions used throughout the Migration Toolkit (MT).

The intent is that every module behaves consistently regardless of its function.

---

# 2. Engineering Principles

Every module shall be:

- Modular
- Profile-driven
- Discover rather than assume
- Read-only by default
- Observable
- Auditable
- Recoverable
- Idempotent where practical

---

# 3. Technology Stack

| Component | Technology |
|-----------|------------|
| Workflow | PowerShell 7+ |
| Database | SQL Server |
| DICOM | DCM4CHE Toolkit |
| Configuration | JSON |
| Documentation | Markdown |
| Logging | JSON + Text |

---

# 4. Standard Repository Layout

```text
Core/
Modules/
Profiles/
SQL/
PowerShell/
Tools/
Reports/
Logs/
Documentation/
Examples/
```

---

# 5. Universal Module Lifecycle

Every executable module follows the same lifecycle.

```text
Initialize
    ↓
Load Profile
    ↓
Discover Environment
    ↓
Validate Inputs
    ↓
Preview / Dry Run
    ↓
Backup (if required)
    ↓
Execute
    ↓
Verify
    ↓
Report
    ↓
Cleanup
```

This lifecycle is mandatory for all execution modules.

---

# 6. Module States

Normal states:

- Created
- Initialized
- Validated
- Ready
- Executing
- Verifying
- Completed

Failure states:

- Validation Failed
- Execution Failed
- Verification Failed
- Aborted
- Rolled Back

---

# 7. Diagnostics Framework

Every module should support progressively richer diagnostics.

| Mode | Purpose |
|------|---------|
| Verbose | Operator-friendly progress |
| Debug | Developer information |
| Diagnostic | Environment inspection |
| Trace | Full execution trace |

Each lifecycle phase should emit PASS/WARN/FAIL status.

Example:

```text
[Initialize] PASS
[Load Profile] PASS
[Discovery] PASS
[Validation] PASS
[Preview] PASS
[Backup] PASS
[Execution] PASS
[Verification] PASS
[Report] PASS
```

---

# 8. Logging

Each module produces:

- Console summary
- Human-readable log
- Structured JSON log
- Optional HTML report

Minimum JSON fields:

- Timestamp
- Module
- Action
- Status
- Duration
- Environment
- Operator

---

# 9. SQL Standards

- Schema-qualified objects
- Transactions for write operations
- Rollback support where practical
- Parameterized SQL
- Avoid unnecessary dynamic SQL
- Idempotent scripts where possible

Naming:

- MT_<Module>_<Action>
- VW_MT_<Name>
- FN_MT_<Name>

---

# 10. PowerShell Standards

Use approved Verb-Noun naming.

Examples:

- Get-MTEnvironment
- Invoke-MTDiscovery
- Start-MTMigration
- Test-MTValidation
- Invoke-MTRecovery

Support:

- -WhatIf
- -Confirm
- -Verbose

when appropriate.

---

# 11. Module Contract

Every module should expose:

- Initialize
- Validate
- Execute
- Verify
- Report
- Cleanup

Return object:

```json
{
  "Module":"Recovery.ExceptionResend",
  "Status":"Success",
  "ItemsProcessed":190,
  "Warnings":[],
  "Errors":[]
}
```

---

# 12. Configuration

Configuration precedence:

1. Command line
2. Environment Profile
3. Module Defaults
4. Global Defaults

No customer-specific values belong in module code.

---

# 13. Error Handling

Errors are classified as:

- Informational
- Warning
- Recoverable
- Critical
- Fatal

Each error should include:

- Message
- Context
- Recommended remediation

---

# 14. Reporting

Each module should produce:

- Summary
- Results
- Warnings
- Errors
- Runtime
- Environment
- Verification outcome

---

# 15. Self-Test

Every module should include a preflight test.

Example:

```powershell
Test-MTModule Recovery.ExceptionResend
```

Checks include:

- Profile validity
- SQL connectivity
- Required tables
- Required stored procedures
- Required tools
- Output locations

---

# 16. Versioning

Semantic Versioning:

- Major
- Minor
- Patch

Each module maintains its own version while MT maintains an overall release version.

---

# 17. Future Enhancements

- Plugin architecture
- REST API
- GUI
- Distributed execution
- AI-assisted recommendations
- DICOMweb
- FHIR

---

# Technical Design Statement

Consistency is a feature. Every MT module should look, behave, execute, diagnose, and report in the same way, reducing operational risk and making troubleshooting significantly easier for migration engineers.

---

## Revision History

| Version | Date | Notes |
|---|---|---|
| 0.1 | 2026-06-29 | Initial draft. |
