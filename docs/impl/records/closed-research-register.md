# Closed Research Register

## Work contract

Direct request that changes the shipped `documentation-integrity` capability. The template lets a
task declare `Research: yes` and accepts a well-supported negative result, but it gives that result
no place to live once the task leaves the plan. Port a downstream project's dedicated register,
`docs/impl/future-research.md`, into the template:

1. Amend "Documentation and planning integrity" in the specification so the register joins the
   document model. Cover what it owns, its boundary, and its evaluation, including a valid negative
   result. Update the registry evaluation and keep the status `shipped`.
2. Create the register with its rules header and an explicit empty state, without seeding it.
3. Add the register to the planning workflow: before opening a research task, in the
   `Research: yes` paragraph, in the completion transition, and for reopening a question.
4. Point the record template's research evidence guidance at the register section.
5. Index the register in `docs/README.md`, `docs/impl/current.md`, and `README.md`, and add one line
   to "Finish work" in `AGENTS.md`.
6. Recommended: add a structural check under `src/agent_py/quality/` with fixtures under
   `tests/quality/`, run from `lint-spec-plan`. If the check does not pay for itself, ship the
   documents alone and record why.
7. Describe the register and any new check in the governance current-state page.

Acceptance: `make ci` and `make quality` pass; fixtures fail for each refused shape and pass for the
empty and valid register; the repository tree passes; `make plan-status` counts are unchanged and
recorded here; `git diff --check` is clean and only intended files change.

## Implementation and decisions

- The specification section was amended first. It covers the register's ownership, its boundary
  (not a backlog, changelog, or copy of the evidence; reopening-condition quality stays reviewer
  judgment; outside the forward-language check), and its evaluation. The executable-contract list
  and success criteria name the register. The registry row stays `shipped` with an updated
  evaluation.
- Section parts are `###` subheadings (`Question`, `Answer`, `Reopening condition`,
  `Do not re-run`) rather than bold inline labels. Heading lines are unambiguous to a parser, give
  each part room for lists, links, and formulas, and match how `plan_model.py` reads plan structure.
  Part names match without case, and extra `###` subsections are allowed. This leaves room for a
  section that also says what carries the operator-facing number instead.
- The owner of a reopening path is a `- Reopening task:` line with backticked IDs, mirroring the
  plan's `- Requires:` field. It may name a task in either lane. When the owning task leaves the
  plan, the check fails, which forces the section to be revisited at that point.
- A reopened question gets a new task id because the closed id stays on the register heading. On
  acceptance the section is removed (shipped behavior) or rewritten under its original heading
  (negative again), so record links to its anchor keep resolving. A removed section's stale links
  surface through `make lint-doc-links`.
- `research_register.py` has its own entry point, which `lint-spec-plan` runs after
  `plan_integrity`. This keeps `plan_integrity.py` at 210 lines and keeps `make plan-status` from
  withholding scheduling because of register state.
- The check does not verify: that the empty-state sentence is removed once sections exist, that a
  section links a task record, or the lane of a reopening task. Each would couple the parser to
  prose or to conventions the request leaves to judgment.
- Beyond the listed scope, one-sentence pointers were added to the plan preamble and the guide
  index, and the `lint-spec-plan` descriptions in the Makefile and README were updated, so every
  index that names the document model or the gate stays accurate.
- Item 6 outcome: the check ships. The parser is 139 lines including the CLI entry point. It reads
  only `##` and `###` heading lines, fences, and one field prefix. The fixtures exposed no brittle
  shape. The one realistic variation found while writing them, title-case part headings, is
  accepted.

## Acceptance evidence

Accepted. Initial `make plan-status` before any edit:

```text
tasks: 1
agent lane: 1
human lane: 0
statuses: CLEAR=1
priority agent task: personalize-template-project [project-identity]
priority human task: none
```

Initial `make ci` passed with 40 tests.

- `tests/quality/test_research_register.py`: 19 tests pass. The empty register (with a fenced
  example section), a complete section that records a date and history words, a human-lane
  reopening task, and title-case part headings all pass. Each refused shape fails with its named
  diagnostic: unbackticked heading, heading without a question, id still open in the plan,
  missing part, renamed part, swapped parts, empty part, reopening task not open, unbackticked or
  empty reopening value, and duplicate section. Exact line attribution and both `main` exit codes
  are asserted. The repository tree passes.
- Failure path at the normal entry point: on a scratch copy with an appended section naming the
  open `personalize-template-project` and an unknown reopening task, the module reported both
  findings with register line numbers and exited 1.
- `make ci`: passed; 59 tests; `[spec-plan] 0 finding(s)`, `[research-register] 0 finding(s)`,
  `[doc-links] 0 broken link(s)`.
- `make quality`: passed; total coverage 96.75% against the 90% floor with `research_register.py`
  fully covered; Markdown lint clean; source and wheel distributions built.
- `make quality-report`: no file over the 250-line soft limit.
- `git diff --check`: clean. No gate was skipped.

Final `make plan-status` matches the initial output exactly.

## Handoff

Plan counts are 1 agent task and 0 human tasks before and after; this change adds and removes no
plan task. The shipped `documentation-integrity` capability changes in place: its registry
evaluation now includes the closed-research register check. No `Requires` or `Dependencies` field
changed. The register starts empty. `personalize-template-project` remains open, and its
repository-wide rename covers the new module and register like any other tracked file.
