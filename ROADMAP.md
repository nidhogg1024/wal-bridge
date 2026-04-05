# Roadmap

## Phase 1: Postgres CDC Core

- Postgres source initialization
- Snapshot for a single table
- WAL incremental reader
- Webhook sink
- Offset persistence

## Phase 2: Pipeline Hardening

- Multi-table filtering
- Restart recovery from checkpoint
- Retry strategy for sink failures
- Better event envelope design
- Basic metrics

## Phase 3: More Sinks and Schema Handling

- Kafka or ClickHouse sink
- Schema change handling
- Delivery diagnostics
- Replay tooling

## Phase 4: Reliability and Performance

- Stress tests
- Backpressure and buffering strategy
- Large snapshot behavior
- Operational playbooks

## Not Planned for Early Versions

- Multi-database support from day one
- Exactly-once guarantees
- Large management UI
- Complex transformation DSL
