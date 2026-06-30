# 3C Migration Engineering Platform

The 3C Migration Engineering Platform (MEP) is the Git-backed engineering platform for medical imaging migration discovery, assessment, planning, execution, validation, recovery, monitoring, reporting, and knowledge capture.

The platform is broader than a single application. It is intended to preserve migration engineering knowledge, standardize repeatable practices, and provide a safe framework for reusable migration tooling.

## Current Stage

- Platform: 3C Migration Engineering Platform (MEP)
- First package: 3C Migration Toolkit (MT)
- Phase: Phase 0 - Project Foundation
- Status: Active Development
- Default branch: develop
- Canonical documentation format: Markdown

## Guiding Statement

Migration is temporary. Engineering capability is permanent.

## Initial Design Principles

1. No one-off scripts.
2. Documentation precedes implementation.
3. Profiles over hardcoding.
4. Safe by default.
5. Observable by design.
6. Capture the lesson.

## Repository Structure

```text
migration-engineering-platform/
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── manifest.json
├── docs/
│   ├── 00 - Project/
│   ├── 01 - ADR/
│   ├── 02 - Engineering Logs/
│   ├── 03 - Case Studies/
│   ├── 04 - Runbooks/
│   ├── 05 - Standards/
│   └── 06 - Releases/
├── src/
│   ├── Core/
│   ├── Discovery/
│   ├── Assessment/
│   ├── Planning/
│   ├── Execution/
│   ├── Validation/
│   ├── Recovery/
│   ├── Monitoring/
│   ├── Reporting/
│   └── Utilities/
├── sql/
├── profiles/
├── tools/
├── examples/
├── templates/
├── tests/
├── releases/
└── assets/
```

## Documentation Model

- MT files are platform specifications and governance documents.
- ADR files capture architecture decisions and reasoning.
- ENG files capture investigation journals and implementation history.
- CS files capture real-world case studies.
- RB files capture operational runbooks.
- MOD files capture module specifications.

## Documentation Rules

- Markdown is the source of truth.
- DOCX and PDF exports are generated artifacts.
- Significant design choices should be captured as ADRs.
- Investigation details should be captured as Engineering Logs.
- Customer or environment-specific values belong in profiles, not core code.
- Production-changing workflows must be safe by default.

## Branching Model

- main is reserved for stable releases.
- develop is the active integration branch.
- feature branches are used for larger units of work when needed.

## Relationship to the Migration Toolkit

The Migration Toolkit (MT) is the first major package within the Migration Engineering Platform. MT provides the initial framework, documentation, profiles, and modules that will grow into the broader platform.
