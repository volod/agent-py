# Current Implementation

This index describes behavior available now. For product intent read the
[specification](../design/spec.md); for work that remains read the [forward plan](plan.md).

## Documentation shape

Small areas stay as one page. When an area grows, its page becomes a short orientation and index,
and focused topics move under `current/<area>/`. New pages must be linked from their area in the same
change. Write behavior, modules, commands, and durable findings at the narrowest level.
[Task records](records/README.md) preserve acceptance evidence.

## Areas

| Area | Owns |
| --- | --- |
| [Developer tooling](current/developer-tooling.md) | Locked setup, Make workflows, CI, quality gates, artifact roots |
| [Product core](current/product-core.md) | Starter package identity and CLI |
| [Governance](current/governance.md) | Agent adapters, capability registry, plan lanes, integrity checks |
