# Autonomous engineering architecture

Discussion supplies intent; agent setup captures it in PROJECT.md and initializes
STATE.md. The two prompts in [README.md](README.md#start-a-project) start this
workflow and resume the engineering loop from repository state.

The engineering coordinator is the active agent's role. It selects each action
from the gap between requirements and observed reality within the current phase.
The project workspace carries memory across agents. Hardware projects follow these
phase boundaries; the engineering loop below governs work inside each phase.

```mermaid
flowchart TD
    R[Requirements] --> SW[Autonomous Hardware-Free Engineering<br/>SOFTWARE_DEVELOPMENT]
    SW --> ST[Simulation / Mocks / Automated Testing]
    ST -->|Software gaps| SW
    ST -->|All meaningful hardware-free work complete| HR[Hardware-Ready Candidate<br/>HARDWARE_READY]
    HR --> SR[Final software-side review and candidate report]
    SR -->|Software gaps| SW
    SR -->|Review prepared| HG[Human Review Gate<br/>AWAITING_HUMAN_REVIEW: save and stop]
    HG -->|Explicit candidate approval with scope and limits| HV[Hardware Validation<br/>HARDWARE_VALIDATION: identify device and integrate]
    HV --> PT{Physical acceptance and final integrated validation pass?}
    PT -->|No| DR[Debug / fix / software regression]
    DR -->|Within approved scope| HV
    DR -->|Review scope or safety assumptions changed| SR
    PT -->|Yes: evidence and report complete| VC[Validated Completion<br/>VALIDATED]
```

The normal hardware path is **Autonomous Hardware-Free Engineering → Hardware-Ready
Candidate → Human Review Gate → Hardware Validation → Validated Completion**.
Missing hardware never stops useful hardware-independent work. At HARDWARE_READY,
prepare the software-side review package described in AGENTS.md; the agent then
saves AWAITING_HUMAN_REVIEW and stops. The gate is a planned boundary, not BLOCKED.
No real-device discovery, reads, initialization, writes, tests, or cleanup occur
before explicit candidate approval. On resume, the gate remains in force; retain
applicable recorded approval rather than repeatedly requesting it.

After approval, confirm device identity and compatibility, prefer the least
consequential useful read, validate initialization/state reporting, then perform
controlled actuation and physical acceptance within limits. Software-only projects
skip the hardware gate and use SOFTWARE_DEVELOPMENT → VALIDATED after their required
tests and final validation pass.

The core loop remains **Inspect → Gap → Design → Implement → Test → Diagnose →
Update State → Repeat**, with evidence deciding the next action:

```mermaid
flowchart TD
    H[Human] --> DS[Discuss objective, behavior, constraints and resources]
    DS --> PS[Agent configures project workspace]

    subgraph W[Project workspace]
        PS --> P[PROJECT.md: objective, criteria, constraints]
        PS --> S[STATE.md: canonical checkpoint]
        P --> C[Engineering coordinator: inspect requirements and current state]
        S --> C
        E[Records and engineering artifacts] --> C
        C --> G[Identify highest-priority gap]
        G --> D[Choose action and design]
        D --> A{Allowed in phase, resources and authority available?}
        A -->|Yes| I[Implement or investigate]
        I --> T[Test / measure]
        T --> V[Diagnose / evaluate]
        V --> U[Update state and evidence]
        U --> S
        U --> E
        U --> Q{Requirements satisfied?}
        Q -->|No: inspect next gap| C
        Q -->|Yes| F[Final validation on final configuration]
        F --> K{Final validation passes?}
        K -->|No: record failure| V
        K -->|Yes| O[outputs/REPORT.md and VALIDATED checkpoint]
    end

    A -->|Not yet| B[Defer action and record dependency]
    Q -->|Only external dependencies remain| B
    B -->|Independent work remains| G
    B -->|Hardware-free work exhausted before integration| RG[Hardware-ready candidate and review gate above]
    B -->|No independent work, genuine blocker outside review gate| BO[BLOCKED report and exact resumption condition]
    BO --> H
    RG --> H
    H -->|Result or explicit scoped candidate approval| U
    C -.-> X[Optional engineering capabilities / subagents]
    X -.-> I
    X -.-> V
    R[External hardware / software / resources] -.->|Documentation| C
    I -.->|Phase-allowed and authorized operations| R
    R -.->|Phase-allowed and authorized tests| T
```

The authority/resource gate applies to **every** external action in implementation
and testing, and is checked again when conditions change. Defer a physical
dependency until meaningful independent work is exhausted, then follow the
candidate review path before requesting hardware access. Human results re-enter
the evidence/state loop; they do not bypass validation. A final-validation failure
returns to diagnosis and the gap loop. BLOCKED preserves a handoff for a genuine
external dependency; AWAITING_HUMAN_REVIEW preserves the deliberate integration
gate. Neither is a success claim or a reason to skip available software work.

Human intent lives in PROJECT.md. STATE.md points to current artifacts, test
methods, and evidence; records preserve reproducible observations and decisions.
The report describes the engineered system, configuration, demonstrated results,
and operation. Optional specialists return bounded artifacts and evidence to the
coordinator, which maintains the single canonical state.
