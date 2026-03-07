# tech-expert-agents

Specialized expert agents that provide deep technical and strategic analysis from distinct perspectives. Each agent is invoked automatically by Claude when the relevant expertise is needed.

## Installation

```
/plugin marketplace add sebmartin/ai-marketplace
/plugin install tech-expert-agents@sebmartin
```

## Agents

| Agent | Purpose |
|-------|---------|
| **Architect** | System design, scalability, component boundaries, data architecture |
| **Security Reviewer** | Threat modeling, vulnerability identification, compliance |
| **Product Strategist** | User value, market fit, prioritization, MVP scope |
| **Tech Advisor** | Technology selection, trade-off analysis, migration paths |
| **Cost Analyzer** | Infrastructure costs, scaling economics, ROI analysis |

## Usage

Agents run automatically when Claude delegates work to them. You can also invoke them explicitly:

```
Use the architect agent to review this design.
Get the security reviewer's take on this auth flow.
What does the cost analyzer say about this approach?
```

## Notes

- All agents use the `opus` model for deep analysis
- Agents have `user` memory scope — they retain context across sessions
- Available tools per agent: `Read`, `Grep`, `Glob` (read-only codebase access)

For general-purpose critical thinking and debate, the `devils-advocate` agent is included in the `ai-workspace` plugin.
