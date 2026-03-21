# Deterministic Ledger

**Document Type:** System Description  
**Status:** Active  

---

## Scope

This document describes the Deterministic Ledger as an append-only public log of declared structural events.

The ledger defines a lightweight mechanism for recording ordered entries referencing repository states and associated artifacts.

The ledger is independent of any specific architectural framework or specification.

It defines its purpose, operational model, and data characteristics within a repository-based publication environment.

## Function

The ledger provides an append-only trace layer over repository states and associated artifacts.

The ledger provides:

- chronological ordering of declared entries  
- deterministic hash references to repository snapshots (when present)  
- explicit UTC timestamp context  
- compatibility with external anchoring mechanisms such as DNS publication  

The ledger does not:

- define architectural models  
- define system semantics  
- define execution semantics  
- provide authorship certification  
- provide legal validation  
- act as a cryptographic authority     

## Operational Model

- Entries are append-only  
- Existing records are never modified  
- Corrections are recorded as new entries  
- Repository state may be referenced via snapshot hashing  

## Snapshot Hash

A snapshot hash is a SHA-256 hash computed over a repository snapshot at a specific commit.

Snapshot hashes are derived from committed repository states only.

Computation:

- the repository state is committed  
- a snapshot of the committed state is produced (e.g., via `git archive HEAD`)  
- a SHA-256 hash is computed over the snapshot  

The resulting hash represents the repository state at that commit.

Snapshot hashes are reproducible when computed using the same process and environment.

No cross-environment reproducibility guarantees are defined.

## Canonical Hash Embedding

After a snapshot hash is computed, it is written into a canonical file (e.g., `CANONICAL_HASH.txt`) and committed.

A new snapshot hash is then computed from the updated repository state.

The published snapshot hash therefore corresponds to a repository state that already includes a previously computed snapshot hash.

## DNS Anchor

DNS is used as an external publication anchor for snapshot hashes.

A DNS anchor consists of:

- a DNS record name associated with a ledger entry  
- a DNS record value containing a SHA-256 snapshot hash  
- a hash derived from a committed repository state after the canonical hash file has been committed

The hash algorithm identifier is part of the published value and MUST be explicitly declared.

Example:

_dledger-dl-0001.zdrahal.eu  
sha256=66cf0b7d998eda3240b9db2e9b113c4686033aa78cd7454c97efcb123de67a51

Properties:

- public visibility  
- independent publication outside the repository  
- explicit reference to a committed repository state  

Limitations:

- DNS does not guarantee immutability  
- DNS does not provide cryptographic timestamping  
- DNS does not act as a trust authority  

## Ordering

The sequence is:

- a ledger record file is created  
- the record file is committed  
- a snapshot hash of the committed repository state is computed  
- the hash is written into the canonical hash file  
- the canonical hash file is committed  
- a new snapshot hash is computed from the updated repository state  
- the resulting hash is published via DNS  

DNS publication therefore always refers to a committed repository state that already includes the relevant ledger record and canonical hash file.

## Recorded Fields

Entries are lightweight and may vary in structure.

Typical fields include:

- Entry-ID  
- UTC timestamp (ISO 8601)  
- Artifact identifier  
- Origin classification (e.g., NEW, ARCHIVAL, DERIVED, IMPORT)  
- Type classification (e.g., Entry-Type, Release-Type)  
- Hash reference (e.g., Snapshot-SHA-256, Canonical-SHA-256)  
- Optional descriptive fields  

Field names and presence are not strictly standardized and may vary across entries.

Entries are primarily structural, but limited descriptive metadata may be included.
