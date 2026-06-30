# 3C Migration Engineering Platform

The 3C Migration Engineering Platform is the Git-backed home for the 3C Migration Toolkit (MT), migration documentation, architecture decisions, engineering logs, runbooks, case studies, profiles, SQL, PowerShell, examples, and release artifacts.

## Current Stage

- Project: 3C Migration Toolkit (MT)
- Phase: Phase 0 - Project Foundation
- Status: Active Development
- Canonical documentation format: Markdown

## Repository Areas

```text
00 - Project/                      Project-level architecture and specifications
01 - Profiles/                     Environment profiles used by MT
02 - Modules/                      Module specifications and module source areas
03 - Case Studies/                 Real-world migration case studies
04 - Runbooks/                     Operational procedures
05 - SQL/                          SQL scripts, templates, and future SQL modules
06 - PowerShell/                   PowerShell orchestration scripts and modules
07 - Examples/                     Example inputs, outputs, and reports
08 - Tools/                        Tooling notes and bundled-tool documentation
09 - Reports/                      Generated or sample reports
10 - Logs/                         Sample logs and log format examples
11 - Templates/                    Document, report, SQL, and PowerShell templates
12 - Profiles (Sample)/            Sanitized sample profiles
13 - Test Data/                    Sanitized test data and fixtures
14 - Releases/                     Release notes and packaged releases
15 - Assets/                       Images, diagrams, logos, and screenshots
16 - Architecture Decision Records/ Architectural decision records
17 - Engineering Logs/             Investigation and implementation journals
99 - Archive/                      Superseded or archived material
```

## Documentation Rules

- Markdown is the source of truth.
- DOCX and PDF exports, when created, are generated artifacts.
- Significant design choices should be captured as ADRs.
- Investigation details should be captured as Engineering Logs.
- Customer or environment-specific values belong in profiles, not core code.
- Production-changing workflows must be safe by default.

## Initial Design Principles

1. No one-off scripts.
2. Documentation precedes implementation.
3. Profiles over hardcoding.
4. Safe by default.
5. Observable by design.
6. Capture the lesson.
