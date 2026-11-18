# Governance

## Agent instructions

[AGENTS.md](../../../AGENTS.md) contains shared rules and points agents to the relevant workflow.
Use repository checks to enforce testable rules and keep detailed procedures in the contributor
guide.

- Codex discovers the root policy and supports discovery of narrower instructions along the
  working-directory path. [Codex instruction discovery](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- Claude Code expands the `@AGENTS.md` import in [CLAUDE.md](../../../CLAUDE.md).
  Use `/memory` to inspect loaded instructions. [Claude Code memory](https://code.claude.com/docs/en/memory)
- Cursor Agent uses the root policy directly. Projects copied from the template can add scoped
  project rules for directory-specific guidance. [Cursor rules](https://cursor.com/docs/rules)
- [GEMINI.md](../../../GEMINI.md) routes Gemini contributors to the shared policy.

## Planning and acceptance

The [specification](../../design/spec.md) owns capability boundaries, evaluations, and registry
order. The [forward plan](../plan.md) owns remaining work. Current-state pages describe available
behavior, and [task records](../records/README.md) preserve the original contract and acceptance
evidence. The [planning workflow](../../guide/planning-workflow.md) covers changes to each document.

`plan_model.py` reads the registry and task metadata. `plan_integrity.py` checks group placement,
lane statuses, required metadata, registry order, and forward-only language.
`plan_dependencies.py` checks open task references across both lanes for unknown IDs, cycles,
and required work that depends on optional work.

`make plan-status` and `agent-py-plan` use `plan_summary.py`. They count work and surface lane
priority after filtering tasks whose explicit prerequisites are still open. The template starts
with `personalize-template-project`. Contributors verify manual prerequisites before taking it.

`make lint-doc-links` uses `doc_links.py` to validate relative links and heading anchors before a
Git commit is required. `make ci` runs both documentation gates and the fixtures in
`tests/quality/`, including invalid task graphs and cross-lane scheduling.

The [workflow update record](../records/agent-workflow-refresh.md) describes the reference review
and verification behind these conventions.
