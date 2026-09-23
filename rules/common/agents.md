# Agent Orchestration

## Available Agents

This is a C#/.NET-only fork: the `ecc@ecc` plugin's own agents (`ecc:planner`,
`ecc:architect`, etc.) stay disabled here to avoid overlapping with the
project-specific roster below, defined in `~/.claude/agents/`:

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| Vulcan-Dispatch | Entry point for C# generation/modification | Any .NET code-gen task; detects Generic/AWS/Azure and routes to the right Vulcan |
| Vulcan-Core / Vulcan-AWS / Vulcan-Azure | Target-specific C# generation | Once Vulcan-Dispatch (or you) has picked the target |
| Vulcan-Patterns | Advanced architectural patterns | CQRS, SignalR, GraphQL and similar, not plain CRUD |
| Vulcan-SCA | NuGet dependency scanning + remediation | SCA sweeps, vulnerable/deprecated/outdated packages |
| Anubis | Structured .NET code review | After writing or modifying C# code |
| Anubis-devops | Azure DevOps YAML pipeline security review | Pipeline changes |
| Anubis-Arch / Anubis-Runtime / Anubis-GreenOps | Architecture governance, performance, cloud cost | Deeper analysis passes, not every review |
| SharpGuard | C# security vulnerability detection + fix | Security-sensitive code (auth, payments, user data) |

## Immediate Agent Usage

No user prompt needed:
1. C# code to generate or modify - Use **Vulcan-Dispatch** (it routes onward)
2. Code just written/modified - Use **Anubis** agent
3. Security-sensitive C# code - Use **SharpGuard** agent
4. NuGet dependency sweep - Use **Vulcan-SCA** agent

Exceptions: skip delegation for purely conceptual questions, for reading or
explaining existing code, or when the user explicitly asks to proceed without
a subagent.

## Parallel Task Execution

ALWAYS use parallel Task execution for independent operations:

```markdown
# GOOD: Parallel execution
Launch 3 agents in parallel:
1. Agent 1: Security analysis of auth module
2. Agent 2: Performance review of cache system
3. Agent 3: Type checking of utilities

# BAD: Sequential when unnecessary
First agent 1, then agent 2, then agent 3
```

## Delegation Completion Contract

Applies to every agent at every depth (parent, child, grandchild):

1. **Your final message IS the deliverable.** Never end your turn with "waiting for background agents" — a spawned task is not a completed task. Ending your turn while children are running orphans their results (completed children cannot notify a parent whose turn has ended).
2. **If you delegate, you own collection.** Wait for results, integrate them, then return. Fire-and-forget delegation is forbidden.
3. **Decompose only when the work cannot fit in one context.** Do not re-delegate a task already sized for a single agent — depth is an outcome, not a plan.

> Rationale: observed failure mode — research agents followed "Parallel Task Execution" above, spawned children, and returned "waiting" as their final answer. All children completed successfully but their results were orphaned. The parallel rule without a completion contract produces zombie tasks.

## Multi-Perspective Analysis

For complex problems, use split role sub-agents:
- Factual reviewer
- Senior engineer
- Security expert
- Consistency reviewer
- Redundancy checker
