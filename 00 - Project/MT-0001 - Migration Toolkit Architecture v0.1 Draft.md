# MT-0001 – Migration Toolkit Architecture

**Document ID:** MT-0001  
**Document Title:** Migration Toolkit Architecture  
**Project:** 3C Migration Toolkit (MT)  
**Version:** 0.1  
**Status:** Draft  
**Phase:** Phase 0 – Project Foundation  
**Author:** Bobby Rufener / 3C Care Systems  
**Created:** 2026-06-29  
**Last Updated:** 2026-06-29  

---

## Revision History

| Version | Date | Author | Notes |
|---|---:|---|---|
| 0.1 | 2026-06-29 | Bobby Rufener / 3C Care Systems | Initial draft architecture document. |

---

## Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| MT-0002 | Technical Design Specification | Defines implementation standards for this architecture. |
| MT-0003 | Project Roadmap | Defines phased delivery plan. |
| MT-0004 | Environment Profile Specification | Defines profile-driven configuration model. |
| MT-0008 | Risk and Safety Model | Defines operational safeguards and production safety controls. |
| CS-0001 | WNM Exception Recovery | First validated case study and proof of concept for recovery workflow. |

---

## 1. Purpose

The 3C Migration Toolkit (MT) is a modular engineering toolkit designed to support the complete lifecycle of enterprise medical imaging migrations.

MT is not intended to be a loose collection of one-off scripts. It is intended to become a reusable engineering platform that standardizes discovery, assessment, planning, execution, validation, recovery, monitoring, and reporting across migration projects.

The toolkit exists to:

- Reduce manual migration engineering effort.
- Improve migration safety and repeatability.
- Capture operational knowledge that would otherwise remain tribal.
- Provide a consistent framework for migration troubleshooting and recovery.
- Support both source-side and destination-side migration validation.
- Enable reusable automation across multiple imaging platforms and customer environments.

---

## 2. Vision

The long-term vision for MT is to become the standard internal migration engineering platform for 3C Care Systems.

MT should allow engineers to approach migration work through repeatable modules instead of ad hoc scripts. Each migration activity should be discoverable, configurable, auditable, and documented.

MT should support both immediate operational needs and future platform expansion, including:

- DICOM source discovery.
- External source inventory collection.
- Migration readiness assessment.
- Batch planning and execution.
- Source-versus-destination validation.
- Exception recovery and targeted resend.
- Migration reporting and dashboards.
- Case study and runbook knowledge capture.

---

## 3. Scope

MT supports the complete migration lifecycle.

```text
Discovery
    ↓
Assessment
    ↓
Planning
    ↓
Execution
    ↓
Validation
    ↓
Monitoring
    ↓
Reporting / Recovery
```

The toolkit is intended to support:

- Outbound migrations from internal archives or VNAs.
- Inbound migrations from external DICOM sources.
- Vendor-provided exception lists.
- CSV-based source inventories.
- DICOM Query/Retrieve-based source inventories.
- SQL-based migration platforms.
- BatchMove or ADAM-style workflows.
- Recovery and resend operations.
- Operational reporting and audit trails.

MT begins with Acuo/ADAM recovery because that workflow has already been validated through the WNM exception recovery effort. The architecture, however, is intentionally broader than Acuo.

---

## 4. Design Principles

The following design principles govern all MT development.

### 4.1 No One-Off Scripts

If a script solves a recurring migration problem, it should become a reusable MT module.

One-off scripts are acceptable only for temporary investigation. Once a process is proven useful, it should be converted into a documented, versioned, reusable component.

### 4.2 Documentation Precedes Implementation

Major modules should have a design document before implementation begins.

Documentation should explain:

- What the module does.
- Why it exists.
- What inputs it requires.
- What outputs it creates.
- What risks it introduces.
- How it should be validated.
- How it can be rolled back or safely stopped.

### 4.3 Profiles Over Hardcoding

Customer-specific or environment-specific values must not be embedded in core logic.

Configuration should live in environment profiles, such as:

```text
WNM-Production.json
BAY-Production.json
BCM-Test.json
NIH-Production.json
```

Profiles may define:

- SQL servers
- Database names
- DICOM AEs
- Route names
- Storage locations
- Migration date windows
- ADAM configuration
- BatchMove defaults
- Customer-specific overrides

### 4.4 Safe by Default

MT should default to read-only or staged behavior.

Safety patterns include:

- Dry-run mode.
- Preview before execution.
- Stage before release.
- Explicit operator confirmation.
- Automatic backups where appropriate.
- Verification before and after write operations.
- Narrow updates scoped by exact run comments or run identifiers.

### 4.5 Observable by Design

Every operation should explain what it is doing and why.

MT operations should produce:

- Human-readable console output.
- Structured logs.
- Diagnostic artifacts.
- Verification summaries.
- Reports suitable for review or handoff.

When something fails, the operator should know which phase failed, what was expected, what was found, and what the recommended next step is.

### 4.6 Capture the Lesson

Every significant migration issue should produce reusable knowledge.

That knowledge may become:

- A case study.
- A runbook.
- A module enhancement.
- A troubleshooting guide.
- A validation rule.
- A safety check.

The WNM exception recovery is the first example of this pattern.

---

## 5. Guiding Operational Principles

### 5.1 Discover Rather Than Assume

MT should discover environment details whenever possible.

Examples include:

- SQL Server version.
- Database names.
- Acuo/ADAM configuration.
- Route GUIDs.
- Host GUIDs.
- Destination AE titles.
- BatchMove configuration.
- Storage locations.
- Existing migration job patterns.

Discovery reduces operator error and improves portability between customer environments.

### 5.2 Explain Every Action

The toolkit should communicate:

- What is about to happen.
- Why it is needed.
- What data will be read.
- What data will be changed.
- What validation will be performed.
- How to reverse or recover if something goes wrong.

MT should be educational, not opaque.

### 5.3 Make Troubleshooting Easier

The toolkit should be built for real-world troubleshooting.

Every module should support:

- Verbose output.
- Diagnostic mode.
- Trace mode.
- Phase-level status.
- Self-test / preflight validation.
- Clear error classification.
- Suggested remediation.

This is especially important because migration issues often involve several interacting systems.

---

## 6. Architecture Overview

MT is organized as a modular platform with shared core services.

```text
3C Migration Toolkit (MT)
│
├── Core
│
├── Profiles
│
├── Modules
│   ├── Discovery
│   ├── Assessment
│   ├── Planning
│   ├── Execution
│   ├── Validation
│   ├── Recovery
│   ├── Monitoring
│   ├── Reporting
│   └── Utilities
│
├── SQL
│
├── PowerShell
│
├── Tools
│   ├── DCM4CHE
│   ├── SQLCMD
│   └── Helper Utilities
│
├── Documentation
│
├── Case Studies
│
├── Runbooks
│
├── Examples
└── Releases
```

The core architecture separates:

- Configuration from code.
- SQL data operations from PowerShell orchestration.
- Module logic from project documentation.
- Customer-specific profiles from reusable framework components.
- Operational case studies from executable modules.

---

## 7. Core Components

### 7.1 Core

The Core contains shared services used by all modules.

Potential core services include:

- Configuration loading.
- Profile validation.
- Logging.
- Diagnostics.
- SQL connection handling.
- DICOM tool invocation.
- Result object formatting.
- Report generation.
- Error handling.
- Manifest loading.

### 7.2 Profiles

Profiles describe environments.

Profiles allow the same module to run against different customers, systems, or migration projects without changing code.

### 7.3 Modules

Modules implement specific migration capabilities.

Modules should be independently executable where possible and composable into larger workflows.

### 7.4 SQL

SQL is the primary data processing layer for database-backed migration activities.

SQL scripts and stored procedures may support:

- Source discovery.
- Reconciliation.
- Validation.
- Batch generation.
- Status monitoring.
- Report staging.

### 7.5 PowerShell

PowerShell is the orchestration layer.

PowerShell should handle:

- Operator interaction.
- Profile selection.
- Workflow execution.
- Module sequencing.
- Logging.
- Report generation.
- External tool execution.
- Safety prompts.

### 7.6 Tools

Bundled tools provide external capabilities.

The most important initial tool is DCM4CHE, which supports DICOM Query/Retrieve and metadata inspection.

### 7.7 Documentation

Documentation is a first-class component of MT.

The documentation set should include:

- Architecture.
- Technical specifications.
- Standards.
- Safety model.
- Module guides.
- Runbooks.
- Case studies.
- Release notes.

### 7.8 Manifest

The `manifest.json` file acts as the project registry.

It tracks:

- Project metadata.
- Documents.
- Modules.
- Profiles.
- Dependencies.
- Case studies.
- Runbooks.
- Releases.

Over time, MT may read the manifest directly to build menus, validate documentation coverage, generate indexes, and support release management.

---

## 8. Module Architecture

### 8.1 Discovery

Discovery modules identify environment configuration and system topology.

Examples:

- SQL Server discovery.
- Database discovery.
- Acuo environment discovery.
- Route discovery.
- AE discovery.
- Storage discovery.
- DICOM endpoint discovery.

### 8.2 Assessment

Assessment modules determine what exists and what needs to be migrated.

Examples:

- External source inventory.
- CSV import.
- DICOM query inventory.
- Source image counts.
- Capacity analysis.
- Duplicate detection.
- Pre-migration gap analysis.

### 8.3 Planning

Planning modules prepare migration scope and execution strategy.

Examples:

- Batch planning.
- Priority planning.
- Date-range planning.
- Customer scope definition.
- Migration profile generation.
- Throughput estimates.

### 8.4 Execution

Execution modules perform controlled migration actions.

Examples:

- ADAM staging.
- BatchMove generation.
- Queue creation.
- Resume migration.
- Pause migration.
- Retry failed jobs.
- Destination-specific execution logic.

### 8.5 Validation

Validation modules verify migration completeness and quality.

Examples:

- Source-versus-destination reconciliation.
- Study count validation.
- Series count validation.
- Image count validation.
- File existence validation.
- Metadata comparison.
- DICOM validation.

### 8.6 Recovery

Recovery modules repair migration gaps and exceptions.

Examples:

- Exception resend.
- Storage-missing review.
- Failed job recovery.
- Controlled restaging.
- Route correction.
- BatchMove correction.
- Queue cleanup.

The WNM exception recovery process is the initial reference case for this module family.

### 8.7 Monitoring

Monitoring modules observe active migration health.

Examples:

- Queue depth.
- BatchMove status.
- Throughput.
- Error counts.
- Images remaining.
- Estimated completion.
- Operational risk indicators.

### 8.8 Reporting

Reporting modules produce operational, technical, and executive outputs.

Examples:

- HTML reports.
- CSV exports.
- Teams summaries.
- Dashboard workbooks.
- Audit summaries.
- Exception reports.

### 8.9 Utilities

Utility modules provide reusable helper functions.

Examples:

- UID normalization.
- CSV parsing.
- Path validation.
- Date window helpers.
- SQL object discovery.
- DICOM command builders.

---

## 9. Technology Stack

### 9.1 SQL Server

SQL Server is used for database-backed migration logic and reconciliation.

Responsibilities include:

- Querying source systems.
- Staging imported data.
- Reconciling source and destination records.
- Validating storage and image counts.
- Preparing migration batches.
- Monitoring migration state.

### 9.2 PowerShell

PowerShell is used for workflow orchestration.

Responsibilities include:

- Running modules.
- Loading profiles.
- Calling SQL.
- Invoking DCM4CHE.
- Capturing logs.
- Generating reports.
- Prompting operators.
- Supporting dry-run and confirmation logic.

### 9.3 DCM4CHE Toolkit

DCM4CHE is bundled or referenced as the primary DICOM command-line toolkit.

Capabilities include:

- `findscu` for DICOM query.
- `movescu` for DICOM move testing.
- `storescu` for send testing.
- `storescp` for receive testing.
- `dcmdump` for metadata inspection.
- `dcmodify` for controlled metadata testing where appropriate.

Bundling DCM4CHE makes MT nearly self-contained for DICOM-side discovery and validation.

### 9.4 JSON

JSON is used for:

- Environment profiles.
- Manifest.
- Module metadata.
- Structured results.
- Tool configuration.

### 9.5 Markdown

Markdown is used for:

- Architecture documents.
- Technical specifications.
- Runbooks.
- Case studies.
- Module guides.
- Release notes.

---

## 10. Knowledge Architecture

MT preserves both executable tooling and operational knowledge.

Knowledge artifacts include:

- Architecture documents.
- Technical design specifications.
- Standards.
- Runbooks.
- Case studies.
- Troubleshooting notes.
- Lessons learned.
- Reports.

Every major migration challenge should be captured as a case study when it teaches a reusable pattern.

Case studies should document:

- Problem.
- Environment.
- Initial assumptions.
- Investigation.
- Root cause.
- Resolution.
- Validation.
- Lessons learned.
- Toolkit changes inspired by the case.

---

## 11. Repository and Folder Structure

The initial OneDrive project structure is:

```text
3C Migration Toolkit (MT)
│
├── 00 - Project
├── 01 - Profiles
├── 02 - Modules
├── 03 - Case Studies
├── 04 - Runbooks
├── 05 - SQL
├── 06 - PowerShell
├── 07 - Examples
├── 08 - Tools
├── 09 - Reports
├── 10 - Logs
├── 11 - Templates
├── 12 - Profiles (Sample)
├── 13 - Test Data
├── 14 - Releases
├── 15 - Assets
├── 99 - Archive
└── manifest.json
```

This structure may evolve as the toolkit matures, but the current organization is intended to separate project governance, reusable modules, executable scripts, examples, and operational artifacts.

---

## 12. Success Criteria

MT will be considered successful when it:

- Reduces manual migration engineering effort.
- Improves migration repeatability.
- Standardizes migration safety practices.
- Provides reusable discovery, validation, and recovery workflows.
- Captures migration engineering knowledge.
- Supports multiple customer environments through profiles.
- Produces auditable outputs.
- Enables controlled, staged production changes.
- Provides clear troubleshooting output.
- Becomes the primary internal migration engineering platform for 3C Care Systems.

---

## 13. Future Expansion

MT is intentionally vendor-agnostic.

Future expansion may include support for:

- Hyland Acuo.
- Mach7.
- Laurel Bridge.
- eRAD.
- Orthanc.
- DICOMweb.
- FHIR.
- Cloud migration services.
- Vendor-specific source exports.
- Automated dashboards.
- GUI-based workflow.
- AI-assisted migration analysis.

---

## 14. Architectural Statement

The 3C Migration Toolkit is designed as a modular engineering platform, not a script collection.

Its architecture is based on reusable modules, profile-driven configuration, production safety, observability, and knowledge capture. The toolkit should support both immediate operational recovery needs and the broader migration lifecycle, allowing 3C Care Systems to preserve hard-earned migration knowledge and apply it consistently across future projects.

---

## Appendix A – Initial Design Principles

1. No one-off scripts.
2. Documentation precedes implementation.
3. Profiles over hardcoding.
4. Safe by default.
5. Observable by design.
6. Capture the lesson.

---

## Appendix B – Initial Phase 0 Deliverables

| Document ID | Title | Status |
|---|---|---|
| MT-0001 | Migration Toolkit Architecture | Draft |
| MT-0002 | Technical Design Specification | Draft |
| MT-0003 | Project Roadmap | Planned |
| MT-0004 | Environment Profile Specification | Planned |
| MT-0005 | Module Development Guide | Planned |
| MT-0006 | Case Study 0001 – WNM Exception Recovery | Planned |
| MT-0007 | Standards and Conventions | Planned |
| MT-0008 | Risk and Safety Model | Planned |
| MT-0009 | Logging and Diagnostics Specification | Planned |
| MT-0010 | Release and Versioning Strategy | Planned |
