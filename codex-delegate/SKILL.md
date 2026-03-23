---
name: codex-delegate
description: "Mandatory delegation for programming tasks: use Codex CLI iteratively — plan first, then execute step by step, continuing sessions as needed. OpenClaw should not implement code itself."
metadata: {"openclaw":{"emoji":"🧑‍💻","requires":{"bins":["codex"]}}}
---

# Codex-First Development Skill

For any programming-related task, OpenClaw MUST delegate execution to Codex CLI. OpenClaw acts as:
- a planner (breaks work into discrete steps),
- a supervisor (executes one step at a time, checks results),
- an integrator (reviews outputs, decides next step),
- a reporter (summarizes what was done and what changed).

OpenClaw MUST NOT implement code directly for programming tasks.

## What counts as programming-related

Assume it IS programming-related if it involves any of:
- writing/modifying code, refactors, tests, linting, formatting
- debugging errors/logs/stack traces
- build tooling, CI, Docker/K8s, shell scripts, SQL migrations
- "how do I implement X?" software requests

If unsure → treat as programming-related.

---

## Core Rule: Codex is Mandatory

For any task involving code (writing, editing, debugging, planning, reviewing, refactoring, scaffolding):

* You MUST invoke:

```bash
codex --full-auto exec "<task prompt>" 2>/dev/null
```

* Never:
  * Write code manually
  * Suggest code without running Codex
  * Mix manual reasoning + partial Codex usage

---

## Execution Modes

### A) Simple Tasks (Single-Step)

If the task is small and self-contained:

* Execute in one Codex call
* Example:

```bash
codex --full-auto exec "Add a function to validate email addresses in utils.js" 2>/dev/null
```

### B) Complex Tasks (Multi-Step)

If the task involves multiple steps, architecture, or uncertainty:

**Step 1 — Planning**

* Generate a plan using Codex:

```bash
codex --full-auto exec "Create a detailed implementation plan for: <task>" 2>/dev/null
```

* Save the plan to the project (e.g. plan.md or /tasks/<feature>.md)

**Step 2 — Breakdown**

* Convert the plan into actionable steps or TODOs

**Step 3 — Iterative Execution**

* Implement each step using Codex:

```bash
codex --full-auto exec resume --last "Implement step 1: ..." 2>/dev/null
```

* Continue with:

```bash
codex --full-auto exec resume --last "Implement next step: ..." 2>/dev/null
```

---

## Conversation Continuity

* For related steps, ALWAYS continue using:

```bash
codex exec resume --last
```

* This preserves context across iterations
* Start a new Codex session ONLY if:
  * The task is unrelated
  * Context becomes harmful or confusing

---

## Output Handling

* Always suppress stderr:

```bash
2>/dev/null
```

* Only stdout matters (final result)

---

## Prompting Rules for Codex

When invoking Codex:

* Be explicit and deterministic
* Include:
  * File paths
  * Expected behavior
  * Constraints
  * Output format

* Avoid vague prompts like:
  * "improve this"
  * "fix bugs"

Prefer:
* "Refactor api.js to extract request validation into a separate module, without changing external behavior"

---

## File System Interaction

Codex is responsible for:

* Creating files
* Editing files
* Writing plans

The agent should:

* Reference expected paths
* Update TODOs
* Validate results
* Ensure outputs are persisted (not just printed)

---

## Validation & Follow-ups

After each Codex execution:

* After each Codex run, verify outputs against requirements (files created, code structure, expected behavior).
* If the task involves UI or APIs, validate behavior using the browser or HTTP requests.
* If unclear or incomplete:
  * Ask Codex follow-up using:

```bash
codex --full-auto exec resume --last "<clarification or fix>" 2>/dev/null
```

* Do NOT manually patch results
* For multi-step tasks, ensure the current step aligns with the original plan before proceeding.

---

## Anti-Patterns (Strictly Forbidden)

* Writing code directly without Codex
* Mixing manual edits with Codex edits
* Skipping planning for complex tasks
* Restarting Codex unnecessarily (losing context)
* Reading stderr output (must be ignored)

---

## Examples

**Single-step:**

```bash
codex --full-auto exec "Create a REST endpoint /health in Express that returns status OK" 2>/dev/null
```

**Multi-step:**

```bash
codex --full-auto exec "Plan a feature: user authentication with JWT in a Node.js app" 2>/dev/null
```

```bash
codex --full-auto exec resume --last "Implement step 1: create user model" 2>/dev/null
```

```bash
codex --full-auto exec resume --last "Implement step 2: login endpoint with JWT" 2>/dev/null
```

---

## Safety Constraints

You MAY run normal local dev commands.
You MUST ask before destructive actions (mass delete, force push, deploy, publish).
