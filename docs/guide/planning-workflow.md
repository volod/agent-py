# Planning Workflow Guide

The documentation lifecycle keeps product intent, future work, and available behavior separate.

## Choose the work contract

- For a named plan task, read the owning specification and current-state page, verify its
  prerequisites, and open a [task record](../impl/records/README.md) before implementation.
- For an ordinary fix or maintenance request, use the request and the existing specification as
  the contract. Create a record when the work needs a durable handoff.
- For a new capability, define the operator problem, boundary, evaluation, and valid negative
  result in the [specification](../design/spec.md#extending-this-specification). Register the
  capability and plan its work before editing production code. Amend the owning section when
  changing an existing product promise.

Review findings against the work contract. Fix in-scope findings, attach concrete follow-up to
the owning open task or create one, and route new capabilities through the specification. Include
observed behavior, impact, and an acceptance check when scheduling an unfixed bug.

## Task lanes

Use **Agent Implementation Tasks** when an agent can reach acceptance using repository fixtures,
deterministic tools, or an authorized non-interactive run:

- `CLEAR`: code, tests, and docs can finish locally.
- `RUN NEEDED`: implementation is deterministic, but acceptance includes a declared heavier run.

Use **Human-Assisted Tasks** when acceptance itself needs a person or authority unavailable to an
agent:

- `BLOCKED BY HUMAN`: an agent can prepare support, but a human-provided artifact gates completion.
- `HUMAN-GATED`: the outcome is human judgment, authorization, private access, or spend approval.

Add `Research: yes` when the path is uncertain and a well-supported negative result is acceptable.
Research tasks still need scope and acceptance criteria.

## Prerequisites and scheduling

Use the optional `Requires` field for open tasks that must leave the plan before work can start.
List backticked task IDs separated by commas. IDs can refer to either lane. The integrity check
rejects missing IDs, cycles, and required tasks that depend on optional work.

```markdown
- Requires: `prepare-fixtures`, `approve-inputs`
```

Use the required `Dependencies` field for external inputs, manual prerequisites, or evidence
needed at acceptance. Use `none.` when there is no additional prerequisite. A human-lane task
should name the specific input, decision, or authorization required and what an agent can prepare.

`make plan-status` skips tasks with an open `Requires` entry, then lists priority in each lane.
Check the `Dependencies` field and the task's acceptance gates before starting.

## Task shape

Task ids are stable lowercase slugs. A task heading may end with `(optional)`. Use this structure:

```markdown
### Capability name -- `capability-id`

#### stable-task-id

Describe the unresolved operator problem in present or future tense.

- Serves: `capability-id` -- [Specification section](../design/spec.md#section)
- Agent status: CLEAR
- Dependencies: none.
- User-visible outcome: State what becomes possible or trustworthy.
- Scope boundary: State what is in scope and explicitly out of scope.
- Data and artifact paths: Name repository-relative or `$DATA_DIR` locations.
- Execution path: Name modules, fixtures, commands, and any declared run.
- Acceptance gates: State deterministic checks and the negative-result rule.
- Documentation target: [Current-state area](../impl/current.md#areas).
```

The plan integrity gate requires every field. Human-lane tasks use a human-lane status. Optional work
follows required work within its capability.

## Ordering

Capability groups follow the registry. Within a group:

1. hard prerequisites;
2. changes to inputs that later evaluation consumes;
3. required work before optional refinements;
4. cheap deterministic work before expensive runs.

The summary prefers required work across capabilities, then uses registry and document order.
If capability priorities change, edit the registry order and move the same groups in both lanes.

## Completion transition

Before reporting completion:

1. Capture the initial counts with `make plan-status` before editing the plan.
2. Run the declared acceptance checks. Record exact commands, results, and any outstanding gate.
   A declared research negative can satisfy acceptance when the evidence supports it.
3. Update current-state docs with interfaces, locations, verification methods, and durable findings.
4. Remove the accepted task and its ID from downstream `Requires` fields. Add links to current
   facts in `Dependencies` where useful. Mark a newly available capability `shipped` and link its current
   documentation.
5. Run `make ci` on the resulting tree, including documentation links and spec-plan integrity.
   Capture the final counts and close the record when all gates pass.
6. Review the diff and report verification, count changes, affected capabilities, and open gates.

Keep routine run evidence in the record and durable implementation facts in current state.
