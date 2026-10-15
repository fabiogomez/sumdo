<!--
Sync Impact Report
Version change: 1.0.0 → 1.0.1 (patch: tech-stack clarification, no principle change)
Modified principles: none
Added sections: none
Removed sections: none
Modified content:
  - Technology Stack & Style Constraints: backend narrowed from "Node.js +
    Express or Python + FastAPI" to "Python + FastAPI exclusively"
Templates requiring updates:
  - .specify/templates/plan-template.md: ✅ no changes needed (Technical
    Context asks the author to state Language/Version per feature; FastAPI-
    only is already consistent with that field)
  - .specify/templates/spec-template.md: ✅ no changes needed (technology-
    agnostic by design)
  - .specify/templates/tasks-template.md: ⚠ still pending from v1.0.0 —
    "Tests are OPTIONAL" line (line 12) still conflicts with Principle III;
    unchanged by this amendment
  - .specify/templates/commands/*.md: N/A (directory does not exist)
Follow-up TODOs:
  - none new; prior TODO(RATIFICATION_DATE) note from v1.0.0 still applies
    (no earlier adopted version existed before 2026-10-15)
-->

# GloberBooks Constitution

## Core Principles

### I. Layered Architecture & Separation of Concerns

The codebase MUST be organized into distinct Controller, Service, and Data
Access layers. Controllers handle HTTP request/response concerns only;
Services own business logic; Data Access modules are the only code permitted
to issue database queries. Dependency injection MUST be used wherever a
module depends on another module's behavior, so dependencies can be
substituted in tests. Every class and module MUST have a single
responsibility — if a module's purpose cannot be described in one sentence,
it MUST be split.

**Rationale**: Strict layering keeps business rules testable without a
database, keeps controllers thin, and prevents data-access concerns from
leaking into services where they become hard to mock or reuse.

### II. RESTful API Design & Consistency

All HTTP endpoints MUST be prefixed with `/api/v1/` and MUST follow RESTful
conventions (resource-based nouns, `GET`/`POST`/`PUT`/`DELETE` mapped to
read/create/replace/delete semantics). Every endpoint MUST return a
consistent response envelope and a standard HTTP status code (see Error
Handling Standards below) — no endpoint may invent its own ad hoc response
shape or status code convention.

**Rationale**: Consistent API shape lets frontend and third-party consumers
build generic request/response handling instead of per-endpoint special
cases, and makes API versioning tractable.

### III. Test-First Discipline & Coverage (NON-NEGOTIABLE)

Unit tests for all business logic MUST be written before a task is
considered finished; a task that adds or changes business logic without
accompanying tests MUST NOT be merged. Tests MUST follow the Arrange-Act-
Assert (AAA) pattern, MUST mock external dependencies (database, network,
third-party APIs), and MUST live alongside the code they test using the
`[filename].test.[ext]` or `[filename].spec.[ext]` naming convention.
Minimum code coverage across the codebase MUST be 80%.

**Rationale**: Business logic is where financial correctness lives for a
books/ledger product; untested logic is the single highest-risk source of
silent, costly bugs. Mocking external dependencies keeps the suite fast and
deterministic.

### IV. Explicit Error Handling & Standard Responses

All asynchronous operations MUST use `async`/`await`; raw callback-style
asynchronous code MUST NOT be introduced. Every error path MUST be handled
explicitly and MUST return a standard HTTP status code. Error response
bodies MUST conform to:

```json
{
  "error": "Human-readable message",
  "code": "MACHINE_READABLE_CODE",
  "timestamp": "ISO-8601"
}
```

Status codes MUST be used as follows: `200` success with data, `201`
resource created, `400` validation errors (with field-specific messages),
`404` resource not found, `500` unexpected server errors.

**Rationale**: A single, predictable error contract lets every client
(frontend, scripts, monitoring) handle failures the same way, and prevents
silently swallowed errors.

### V. Security & Data Protection

Secrets, API keys, and credentials MUST NEVER be committed to the
repository or hardcoded in source. Passwords MUST NEVER be stored in plain
text. All user input MUST be validated at the boundary before it reaches
services or data access code. Production code MUST NOT use `SELECT *`;
queries MUST name the columns they need.

**Rationale**: GloberBooks handles financial and personal data; credential
leaks, injection via unvalidated input, and over-fetching via `SELECT *` are
the most common and most damaging classes of incidents for this kind of
system.

## Technology Stack & Style Constraints

**Stack**: Backend is Python + FastAPI exclusively; persistence is
PostgreSQL; frontend is React. Introducing a new third-party dependency,
modifying the database schema, or adding a new entity/table requires
asking the user first (see Governance).

**Style**: Variables and functions use camelCase; classes use PascalCase.
Identifiers MUST be descriptive and at least 3 characters long — single-
and double-letter names are not acceptable outside of loop counters in
tightly scoped, obviously-correct loops. Functions MUST NOT exceed 50
lines; files MUST NOT exceed 300 lines — once either limit is hit, the unit
MUST be split along its responsibilities (reinforcing Principle I). Every
public interface (exported function, class, or API route handler) MUST
have a comment describing its contract.

## Change Control & Workflow Boundaries

The following actions require explicit user confirmation before
proceeding, even mid-task:
- Adding any new third-party dependency or library
- Modifying the database schema
- Adding new entities/tables
- Changing an established architectural pattern (e.g., the layering in
  Principle I)

The following are prohibited outright, with no exception path:
- Committing secrets or exposing credentials
- Hardcoding secrets, API keys, or sensitive data in code
- Skipping tests for business logic
- Using `SELECT *` in production code
- Storing passwords in plain text
- Adding features not described in the active `spec.md`

## Governance

This constitution supersedes any conflicting convention, prior practice, or
ad hoc agreement for GloberBooks development. All plans, specs, tasks, and
pull requests MUST be checked against it; any deviation MUST be justified
in writing (e.g., in a plan's Complexity Tracking section) or rejected.

**Amendment procedure**: Amendments are proposed by editing this file,
documenting the change and rationale, and securing explicit user approval
before the new version is adopted. On approval, the version is bumped per
the policy below, `Last Amended` is set to the date of approval, and a Sync
Impact Report is prepended as an HTML comment summarizing what changed and
which dependent templates were checked or updated.

**Versioning policy**: Semantic versioning applies to this document —
MAJOR for backward-incompatible governance or principle removals/
redefinitions, MINOR for new principles or materially expanded guidance,
PATCH for clarifications and non-semantic wording fixes.

**Compliance review**: Every `/speckit-plan` run MUST execute the
Constitution Check gate against the principles above before research
begins and again after Phase 1 design. Reviewers MUST treat the NEVER DO
list in Change Control & Workflow Boundaries as blocking, not advisory.

**Version**: 1.0.1 | **Ratified**: 2026-10-15 | **Last Amended**: 2026-10-15
