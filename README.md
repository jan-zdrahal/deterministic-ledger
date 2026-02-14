# Deterministic Ledger

The Deterministic Ledger is an append-only public log of declared structural events.

It records artifact declarations and state transitions within the deterministic stack environment.

## Function

The ledger provides:

- Chronological ordering of declared entries.
- Deterministic hash references.
- Explicit UTC timestamp context.
- Explicit linkage to external manifest anchors when applicable.

The ledger does not:

- Define stack architecture.
- Disclose domain construction mechanisms.
- Provide authorship certification.
- Provide legal validation.
- Act as a cryptographic authority.

## Operational Model

- Entries are append-only.
- Existing records are never modified.
- Corrections are recorded as new entries.
- Repository state may be cryptographically anchored via deterministic snapshot hashing when explicitly declared.

## Recorded Fields

Each entry contains:

- Entry-ID
- UTC timestamp (ISO 8601)
- Artifact identifier
- Origin classification (e.g., NEW, ARCHIVAL, DERIVED, IMPORT)
- Entry type
- Canonical hash reference (when applicable)
- Optional manifest anchor reference

No interpretative commentary is included.

## Status

Active
