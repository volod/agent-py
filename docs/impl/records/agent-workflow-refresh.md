# Agent Workflow Refresh

## Work contract

Refresh the Python template's agent setup, specification and planning workflow using both
reference repositories and official agent documentation.

## Implementation and decisions

- `ss-track` at `49214bb` uses records to preserve task contracts and acceptance evidence.
  This starter uses task IDs as filenames and links durable decisions from the owning page.
  Cross-capability and per-change reviews happen as needed within the work contract.
- `ss-fusion` at `75debcf` distinguishes start prerequisites from context and acceptance
  inputs. The template uses a dedicated `Requires` field for open task IDs and retains
  `Dependencies` for prerequisites contributors verify directly.
- [Codex instruction discovery](https://learn.chatgpt.com/docs/agent-configuration/agents-md),
  [Claude Code memory](https://code.claude.com/docs/en/memory), and
  [Cursor Agent](https://cursor.com/docs/agent/overview) with
  [Cursor rules](https://cursor.com/docs/rules) establish the adapters and loading behavior.

## Acceptance evidence

Accepted. The locked bootstrap and full quality gate ran with a writable offline uv cache.
`make ci` passed all gates and 40 tests. `make quality` measured 96.05% coverage and built
the source and wheel distributions. `make plan-status` reported the personalization task as the
agent priority with no human task. Agent loading behavior was checked against vendor documentation.

## Handoff

Plan counts are 1 agent task and 0 human tasks before and after. The shipped
`documentation-integrity` capability changes in place, and the personalization task remains open.
