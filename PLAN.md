# Project Plan

## Vision

Build a learning-focused distributed append-only log in Go to explore
durability, replication, consensus, crash recovery, and failure handling.

## Architecture

Client
  ↓
Raft Leader
  ├── Replicated Segmented WAL
  │     ├── Checksums
  │     ├── Indexing
  │     └── Recovery and Truncation
  ├── Raft Consensus
  │     ├── Followers
  │     └── Snapshots
  └── KV State Machine
        ├── SET
        ├── DELETE
        └── CAS (optional)

Committed log entries are the authoritative history. The KV state machine is
derived by applying committed entries, so nodes that replay the same committed
prefix converge to the same state.

## Roadmap

### Phase 1 — System Design
- [ ] Define crash and network failure model
- [ ] Define durability, acknowledgement, and read-consistency guarantees
- [ ] Design WAL record format
- [ ] Define recovery invariants
- [ ] Define module boundaries and interfaces

### Phase 2 — Single-Node Segmented WAL
- [ ] Append records
- [ ] Read records
- [ ] Deterministic record encoding
- [ ] Checksums
- [ ] Explicit `fsync` semantics
- [ ] In-memory indexing
- [ ] Segment rotation
- [ ] Crash recovery
- [ ] Detect incomplete or corrupt tail records
- [ ] Truncate corrupted tail

### Phase 3 — In-Memory Raft Core
- [ ] Leader election
- [ ] Terms and voting
- [ ] AppendEntries
- [ ] Log replication
- [ ] Conflict resolution
- [ ] Commit advancement
- [ ] Apply committed entries

### Phase 4 — Persistent Raft and KV State Machine
- [ ] Persist Raft terms and votes
- [ ] Back Raft with the WAL
- [ ] Implement deterministic KV state machine
- [ ] Implement `SET`
- [ ] Implement `DELETE`
- [ ] Evaluate optional `CAS`
- [ ] Define client acknowledgement semantics
- [ ] Define linearizable and stale-read behavior

### Phase 5 — Recovery and Cluster Features
- [ ] Follower catch-up
- [ ] Restart and crash recovery
- [ ] Snapshots
- [ ] Log compaction
- [ ] Snapshot installation
- [ ] Network partition handling
- [ ] Fault injection

### Phase 6 — Observability and Evaluation
- [ ] Metrics
- [ ] Structured diagnostics
- [ ] Reproducible benchmarks
- [ ] Failure experiments
- [ ] Performance analysis
- [ ] Document correctness results and trade-offs

## Current Focus

Phase 1: System design and failure model

### Tasks

- [ ] Document crash-fault assumptions
- [ ] Document unreliable-network assumptions
- [ ] Specify durability versus commitment guarantees
- [ ] Specify WAL record and segment formats
- [ ] Record recovery invariants
- [ ] Identify initial storage and Raft interfaces
- [ ] Add design decisions to `docs/adr/`

## Next

After the failure model, guarantees, and interfaces are documented, implement
the single-node segmented WAL with checksums, indexing, explicit `fsync`, crash
recovery, and tail truncation.

## Key Decisions

- Raft rather than a custom consensus algorithm
- Implement the storage engine from scratch
- Use the log as authoritative history and derive application state from it
- Keep durability and commitment as separate, explicit guarantees
- Prioritize correctness under failure over feature count
- Let abstractions emerge from concrete use cases
- Validate important decisions with tests, failure scenarios, or measurements
- Keep Byzantine consensus, sharding, distributed transactions, SQL, and a
  general-purpose database engine out of scope
