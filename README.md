# Logable

Logable is a learning-focused distributed systems project in Go: a crash-safe, distributed append-only log built from first principles.

The project combines a custom segmented write-ahead log (WAL) with a from-scratch Raft implementation. A simple key-value state machine will demonstrate how committed log entries become replicated application state.

## Architecture

```text
Client -> Raft leader -> Replicated log -> Commit -> KV state machine
                         |             |
                      followers     snapshots
```

The log is the authoritative history. Application state is derived by applying committed entries, so nodes that replay the same committed prefix should converge to the same state.

## Core goals

- Implement Raft correctly: elections, terms, voting, replication, conflict resolution, and commit advancement.
- Build a segmented WAL with deterministic records, checksums, recovery, truncation, rotation, indexing, and explicit `fsync` semantics.
- Make durability, client acknowledgements, and read consistency explicit and testable.
- Explore snapshots, log compaction, follower catch-up, crash recovery, network partitions, and fault injection.
- Measure performance with reproducible benchmarks instead of optimizing by assumption.

## Initial scope

Development starts with a single-node WAL, followed by an in-memory Raft core, persistent Raft, and a small KV state machine (`SET`, `DELETE`, and optionally `CAS`). Snapshots, compaction, fault injection, observability, and performance work will follow incrementally.

The first version focuses on crash faults and unreliable networks. Byzantine consensus, cryptocurrency features, sharding, distributed transactions, SQL, and a general-purpose database engine are out of scope.

## Design principles

1. Correctness under failure comes before feature count.
2. Implement classical Raft faithfully; original engineering work belongs around storage and system behavior.
3. Abstractions should emerge from concrete use cases.
4. Durability and commitment are different guarantees and must not be conflated.
5. Important decisions should have tests, failure scenarios, or measurements behind them.

## Planned phases

1. System design: failure model, consistency guarantees, WAL format, recovery, and module boundaries.
2. Single-node segmented WAL: append, read, indexing, rotation, checksums, `fsync`, recovery, and truncation.
3. In-memory Raft core.
4. Persistent Raft backed by the WAL.
5. KV state machine and client semantics.
6. Snapshots, compaction, and follower recovery.
7. Fault injection, observability, and reproducible benchmarks.

## Correctness invariants

- Committed entries are never lost.
- Two nodes never apply different commands at the same committed index.
- `lastApplied <= commitIndex <= lastLogIndex`.
- Log indexes are monotonically increasing.
- State machine application is deterministic.
- An incomplete or corrupt WAL tail never becomes a valid entry.

Detailed design decisions will be recorded in `docs/adr/` as implementation develops.
