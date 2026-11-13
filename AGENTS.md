# Agent operating instructions

Applies to every session and task in this repository. Read this file before acting.
Follow higher-priority platform instructions and the user's explicit directions;
apply more specific directory instructions within their scope. Keep this file
tool-neutral and maintain one source of truth for agent guidance.

## 1. Start here and explore narrowly

1. Read `CONTINUITY.md` at the start of each assistant turn before taking action.
2. Read `docs/navigation.md` before broad exploration, including delegated exploration.
3. Follow known entry points or feature roots, then use targeted `rg` content searches.
4. Read the files you intend to edit and relevant callers/contracts before changing them.
5. Produce a short plan before code changes, with a verification check for each step.

Before the first plan, use at most **8 Read, 4 Grep, and 2 Bash calls** (or their
equivalents in another tool). Treat file reads, content searches, and shell
invocations as those categories; batching must not hide extra reads or searches.
These are ceilings, not targets. If more investigation is needed, state the plan
and the specific uncertainty first. Avoid broad filesystem scans and repeated reads.

If a startup file is missing, state that fact and use known context; do not invent
its contents. Restore missing guidance within the task's authorized scope.

### Optional MCP fallback

- Never start a task with MCP queries. Read `docs/navigation.md` first.
- Use `codebase-memory-mcp_search_graph` only to confirm relationships or locate
  cross-feature links after entry points and targeted searches prove insufficient.
- Do not use MCP for simple file lookups or repeat equivalent queries.
- Use at most **2 MCP queries per task** unless you explain why more are necessary.
- MCP is optional. Confirm important findings against current repository files.

## 2. Think before implementing

- Define the goal and observable success criteria before implementation.
- State material assumptions, uncertainties, and tradeoffs plainly.
- Resolve uncertainty from available evidence first. Ask before dependent work when
  ambiguity materially changes scope, data, architecture, or user behavior.
- For routine, reversible choices, state a reasonable assumption and proceed.
- If the goal and requested approach conflict, explain the conflict. Push back on
  unnecessary complexity and present the simpler viable approach.
- Do not fabricate paths, APIs, dependencies, results, or completed checks.
- Finish authorized work and verify it; do not stop at a proposal or plausible diff.

For nontrivial tasks, use a brief plan such as:

1. Identify the affected behavior → verify the current contract or reproduction.
2. Implement the smallest complete change → verify targeted behavior.
3. Align documentation → verify checks and review the final diff.

Use a plan tool when available for short-term execution scaffolding (3–7 steps).
Otherwise keep the plan in the response. Keep its intent consistent with the ledger.

## 3. Project baseline

The product is a recipe cookbook with a website and installable PWA experience for
desktop and especially mobile phones. Intended capabilities include personal
recipes, custom labels, optional folders, measurement choices, images, attached
voice recordings, and spoken recipe transcription.

These describe product intent, **not an approved implementation scope or MVP**.
The next planning work is a PRD and supporting technical documents. Do not choose
a stack, backend, transcription provider, native app strategy, or offline/sync
policy implicitly. Record accepted choices and their tradeoffs in project docs.

Design UI for the end user, not the storage schema. Prioritize mobile use,
responsive desktop layouts, accessibility, and clear recipe entry and reading.
When relevant, explicitly design PWA installation, updates, offline behavior,
media permissions, and user-data persistence; do not assume they come for free.
Preserve entered recipe quantities and units. Any conversion, scaling, or
transcription behavior needs explicit rules and verification in its feature task.

The stack, source roots, package manager, commands, and deployment target are
currently undecided. Once established, document verified commands and entry points
in `docs/navigation.md`; never invent commands or report unrun checks as passing.

## 4. Simplicity and modularity

- Build the minimum complete solution to the requested problem. No speculative
  features, single-use abstractions, unused hooks, or unrequested configurability.
- **No code file may exceed 300 lines of code.** Keep authored source and tests
  modular. Documentation and plans are exempt. Do not hand-edit generated or
  vendored files to satisfy the limit; use their source or generation process.
- Do not shrink functionality or compress code to evade the limit. Split by clear
  responsibility into cohesive files/modules/functions before reaching it.
- Keep entry points and public contracts stable; isolate logic from the start.
  Plan for known requirements without building speculative infrastructure.
- If a simpler implementation solves the same problem, prefer it.
- Reuse suitable open-source, self-hostable libraries instead of reinventing
  established capabilities. For a new dependency or service choice, present
  suitable options and tradeoffs and involve the user in selection before adoption
  unless an existing decision already authorizes it.

## 5. Surgical changes and visible failures

- Every changed line must support the request. Match existing conventions.
- Do not reformat, refactor, or clean up unrelated code. Mention relevant
  pre-existing dead code without deleting it unless asked.
- Remove imports, variables, and functions made unused by your own changes.
- Preserve unrelated user work. Do not discard changes or run destructive cleanup.
- During development, do not add default fallbacks that hide broken behavior,
  missing configuration, failed integrations, or invalid state. Fail visibly and
  fix the cause; never silently substitute mock data or pretend success.
- No empty `try`/`catch`, swallowed exceptions, or catch-and-continue behavior.
  Handle real expected failures with actionable errors. Deliberate product
  recovery behavior must be explicit, documented, and verified.
- Never commit credentials or expose sensitive recipe/media data in logs.

## 6. Verification and completion

- Choose checks that prove the success criteria, proportionate to the change.
- For a bug, reproduce it and add a regression test where practical. For validation,
  check relevant invalid inputs. For refactoring, verify behavior before and after.
- Prefer focused checks during iteration. Run applicable required tests, lint,
  type checks, and build checks before completion once those tools exist.
- Inspect actual outputs. Fix causes instead of weakening checks to make them pass.
- For UI work, inspect the result at mobile and desktop sizes and verify the
  affected interactions, keyboard access, and states. For PWA changes, verify the
  affected installation, offline, update, or persistence behavior explicitly.
- Do not add tests that merely mirror implementation or burden trivial edits.
  Documentation-only tasks need consistency and reference checks, not app tests.
- If verification is blocked, name the unverified behavior and exact blocker.
  Never imply it passed. After repeated failed attempts, reassess the evidence and
  approach; ask for missing input only when it prevents further useful progress.
- Review the final diff for scope, accidental edits, and file size limits.

## 7. Keep documentation aligned

- Update docs when feature behavior, analytics/events, or application flows change.
- Prefer `docs/feature-map.md` for feature-level behavior, flows, events, and links.
- Update `docs/architecture.md` for patterns, persistence, or integration changes.
- Update `docs/navigation.md` when entry points, commands, or document locations change.
- Keep docs aligned in the same task whenever possible. Create the relevant document
  when needed; distinguish proposals from accepted decisions and implemented facts.
- Document event payloads without including secrets or raw personal media/content.
- Keep durable architecture decisions in project docs and summarize them in the ledger.

## 8. Continuity ledger

`CONTINUITY.md` is the canonical compaction-safe briefing for this workspace.
Do not rely on earlier chat or tool output for durable context unless reflected
there. Current explicit user directions and verified repository state supersede
stale ledger entries; record that supersession.

Update it only for meaningful changes to goals/success criteria, constraints,
decisions, Done/Now/Next state, open questions, working set, or important outcomes.

- `Snapshot`: at most 25 lines, including Goal, Now, Next, and Open Questions.
- `Done (recent)`: at most 7 bullets.
- `Working set`: at most 12 paths.
- `Receipts`: keep the last 10–20 entries once available; do not invent filler.
- Every entry must include a date or ISO timestamp and provenance tag:
  `[USER]`, `[CODE]`, `[TOOL]`, or `[ASSUMPTION]`.
- Facts only, no transcripts or raw logs. Mark unknowns `UNCONFIRMED`.
- Record durable choices as ADR-lite entries, e.g. `D001 ACTIVE: ...`.
  Explicitly supersede changed decisions instead of silently rewriting history.
- For recurring incidents, keep a compact capsule with Symptoms, Evidence pointers,
  Mitigation, and Status, with the same date/provenance convention.
- Compress older items into milestones with actual document/commit/PR/log pointers
  when caps are reached. Keep the ledger a briefing, not a micro-task checklist.

Start replies with a brief **Ledger Snapshot**: Goal, Now, Next, Open Questions.
Print the full ledger only when it materially changed or the user requests it.

## 9. Communication and maintenance

- Be direct, concise, and candid. No flattery, filler, invented certainty, or
  unsupported claims. Explain material risks and tradeoffs in plain language.
- Final replies should cover what changed, verification, and remaining blockers.
- Do not re-request decisions or permissions already granted for the same scope.
- Keep these instructions short enough to follow. When a correction exposes a
  recurring gap, add or tighten one concrete rule; do not accumulate duplicate rules.
- Keep any additional agent-tool instruction files aligned with this file rather
  than maintaining conflicting copies. Do not assume all tools discover it automatically.
