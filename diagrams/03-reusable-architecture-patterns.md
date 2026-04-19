# Reusable Architecture Patterns You Can Lift Into Any Project

This diagram is tech-agnostic so you can reuse it as a baseline architecture in your own systems.

```mermaid
flowchart TD
    subgraph Clients[Client Interfaces]
      C1[CLI]
      C2[API or MCP]
      C3[Scheduler or Worker Trigger]
    end

    subgraph Boundary[Unified Contract Boundary]
      B1[Operation Registry]
      B2[Typed Input Schema]
      B3[Shared Error Model]
      B4[Trust Context Local vs Remote]
    end

    C1 --> B1
    C2 --> B1
    C3 --> B1
    B1 --> B2
    B1 --> B3
    B1 --> B4

    subgraph Core[Core Domain Services]
      D1[Write Pipeline parse transform validate]
      D2[Read Pipeline retrieval rank dedup]
      D3[Relationship Graph Builder]
      D4[Background Job Orchestrator]
      D5[Sync and Change Detection]
    end

    B1 --> D1
    B1 --> D2
    B1 --> D3
    B1 --> D4
    B1 --> D5

    subgraph Ports[Ports and Adapters Layer]
      P1[Storage Port Interface]
      P2[Search Port Interface]
      P3[Queue Port Interface]
      P4[External API Port Interface]
    end

    D1 --> P1
    D2 --> P1
    D2 --> P2
    D4 --> P3
    D1 --> P4

    subgraph Infra[Infrastructure Implementations]
      I1[Embedded Local Store]
      I2[Managed SQL Store]
      I3[Vector and Keyword Indexes]
      I4[Durable Queue Backend]
      I5[Object Storage]
    end

    P1 --> I1
    P1 --> I2
    P2 --> I3
    P3 --> I4
    P4 --> I5

    subgraph Reliability[Operational Reliability]
      R1[Schema Migrations]
      R2[Health Doctor Checks]
      R3[Repair Commands]
      R4[Audit and Version History]
      R5[Idempotency and Retries]
    end

    D1 --> R4
    D4 --> R5
    I2 --> R1
    I2 --> R2
    I2 --> R3

    subgraph Quality[Quality Guardrails]
      Q1[Unit Tests]
      Q2[Integration and E2E Tests]
      Q3[Regression Scripts and Static Guards]
      Q4[Benchmark and Eval Harness]
    end

    Boundary --> Q1
    Core --> Q2
    Infra --> Q3
    D2 --> Q4

    subgraph Principles[Design Principles]
      Z1[Single Contract Many Interfaces]
      Z2[Deterministic First AI Optional]
      Z3[Pluggable Infrastructure]
      Z4[Observable and Repairable by Design]
      Z5[Security by Caller Context]
    end

    B1 --> Z1
    D2 --> Z2
    Ports --> Z3
    Reliability --> Z4
    B4 --> Z5
```

## How to apply this quickly

1. Start with an operation registry and run every entry point through it.
2. Make your core services depend on interfaces, not database SDKs.
3. Keep write and read pipelines explicit and stage-based.
4. Add a trust-context flag so local and remote calls can share logic safely.
5. Ship doctor, repair, and migration commands from day one.
6. Treat evaluation and regression checks as architecture, not optional tooling.
