# Meta Search Resolver

A collection of numbered design notes and code sketches for a modular meta-search and identity/account resolver. The repository currently contains `1.txt` through `22.txt` plus this README. Treat the numbered files as an evolving design history until their overlapping proposals are reconciled into one current specification and tested implementation.

## Intended capability areas

The notes discuss:
- query generation and search URL construction;
- pluggable search-engine and site-specific connectors;
- fetching, parsing, normalization, and candidate ranking;
- account/profile identifiers and evidence collection;
- cross-source candidate resolution;
- a possible CLI, API, and browser interface.

These are themes in the design material, not a claim that every connector works or that a production-ready resolver is implemented in this repository.

## Recommended architecture

Separate the system into independently testable components:

1. **Query planning** — build bounded queries from user-provided search terms and authorized scope.
2. **Connector registry** — each connector declares supported operations, authentication needs, terms/limits, and output schema.
3. **Retrieval** — use documented APIs or permitted public interfaces where possible; apply timeouts, rate limits, response-size limits, and error isolation.
4. **Parsing and normalization** — retain raw source evidence and store normalized values separately.
5. **Candidate generation** — group records based on explainable signals without presuming identity.
6. **Evidence assessment** — record which source supports each field, conflicts, retrieval time, and confidence limitations.
7. **Human review and resolution** — keep candidate matches separate from confirmed links; provide an abstain/uncertain outcome.
8. **Audit and export** — preserve query, connector, source URL, retrieval timestamp, transformation version, and reproducible result metadata.

## Identity-resolution safeguards

A shared name, username, avatar, location, or text fragment is not enough by itself to establish that two profiles belong to the same person. The resolver should:
- distinguish account identity from real-world identity;
- require multiple independent and context-appropriate signals for consequential conclusions;
- preserve contradictory evidence and missing fields;
- avoid inventing values or assigning IDs by positional pairing of unrelated parser results;
- expose the basis and limits of every candidate score;
- support explicit review, correction, and rejection;
- respect provider terms, privacy expectations, access controls, and applicable law.

Do not use a numerical score as a probability unless it has been calibrated and validated against an appropriate test set.

## Consolidation plan

- [ ] Inventory all 22 notes and label each as current specification, alternative proposal, example, or superseded draft.
- [ ] Select one canonical architecture and move reusable components into discrete files.
- [ ] Define shared models for `Query`, `Connector`, `SourceObservation`, `AccountCandidate`, `Evidence`, `ResolutionAssessment`, and `AuditEvent`.
- [ ] Define connector interfaces and error/result contracts.
- [ ] Replace brittle HTML scraping sketches with supported APIs or clearly marked, policy-compliant alternatives.
- [ ] Add fixture-based parser tests and deterministic scoring tests.
- [ ] Add rate limiting, timeouts, safe URL handling, and secret management before network execution.
- [ ] Document known gaps, supported connectors, and the exact steps needed to run the project.

## Related projects

- [records-search-engine](https://github.com/bradgeigermanager-sketch/records-search-engine) — source indexing, retrieval, provenance, and reviewed cross-repository navigation.
- [Data-Source-Mapping-And-Overlap-Comparison-Suite](https://github.com/bradgeigermanager-sketch/Data-Source-Mapping-And-Overlap-Comparison-Suite) — source comparison and overlap analysis.
- [Records-Deduplication-Suite](https://github.com/bradgeigermanager-sketch/Records-Deduplication-Suite) — record-level relationship and duplicate assessment.

These are candidate integration points, not existing runtime dependencies.

## Current status

This README provides a map and consolidation plan. It does not certify the numbered examples as executable, current, legally permitted, or safe for production. Validate each connector independently before enabling network access.