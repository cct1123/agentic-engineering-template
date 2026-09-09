# Autonomous engineering architecture

The engineering coordinator is the active agent's role. It selects each action
from the gap between requirements and observed reality, rather than following a
fixed implementation sequence. The project workspace carries memory across agents.

```mermaid
flowchart TD
    H[Human] --> P[PROJECT.md: objective, criteria, constraints]

    subgraph W[Project workspace]
        P --> C[Engineering coordinator: inspect requirements and current state]
        S[STATE.md: canonical checkpoint] --> C
        E[Records and engineering artifacts] --> C
        C --> G[Identify highest-priority gap]
        G --> D[Choose action and design]
        D --> A{Resources and authority available?}
        A -->|Yes| I[Implement or investigate]
        I --> T[Test / measure]
        T --> V[Diagnose / evaluate]
        V --> U[Update state and evidence]
        U --> S
        U --> E
        U --> Q{Requirements satisfied?}
        Q -->|No: next gap| G
        Q -->|Yes| F[Final validation on final configuration]
        F --> K{Final validation passes?}
        K -->|No: record failure| V
        K -->|Yes| O[outputs/REPORT.md and COMPLETE checkpoint]
    end

    A -->|Blocked / controlled action| B[Save blocker and exact human action]
    Q -->|Only external dependencies remain| B
    B --> H
    H -->|Result or scoped authorization| U
    B -->|Independent work remains| G
    B -->|No independent work| BO[BLOCKED report and resumption condition]
    C -.-> X[Optional engineering capabilities / subagents]
    X -.-> I
    X -.-> V
    R[External hardware / software / resources] -.-> C
    I -.->|Authorized operations| R
    R -.->|Authorized tests and observations| T
```

The authority/resource gate applies to **every** external action in implementation
and testing, and is checked again when conditions change. A blocked request does
not prevent independent local work. Human results re-enter the evidence/state
loop; they do not bypass validation. A final-validation failure also returns to
diagnosis and the gap loop. BLOCKED preserves a handoff, not a success claim.

Human intent lives in PROJECT.md. STATE.md points to current artifacts, test
methods, and evidence; records preserve reproducible observations and decisions.
The report describes the engineered system, configuration, demonstrated results,
and operation. Optional specialists return bounded artifacts and evidence to the
coordinator, which maintains the single canonical state.
