# Inspiration Map: *Learn Harness Engineering* → *Build Your Own Agent Harness*

Notes on what I took from the original course in this repo (`docs/en/lectures/`, `projects/`), and what I kept, changed or added.

**Main difference:** the original course teaches harnesses as **files you give to an existing agent** (Claude Code / Codex). My course teaches them as **code you write in your own agent loop**, so each idea can be turned off and measured.

---

## Lecture-by-lecture

| Original lecture | Key idea | Where it lives in my course | Kept / changed |
|---|---|---|---|
| L01 Why capable agents still fail | 4-step failure pattern: incomplete context → locally reasonable changes → no global verification → premature completion. "Fix the harness first." | Failures F1–F4; Module 1 baseline | **Kept** as the north star. Extended with F5–F9. |
| L02 What a harness actually is | Agent = Model + Harness; 5 subsystems (instructions, tools, environment, state, feedback); ablation test | Core mental model; Module 14 | **Changed**: 7 subsystems (added *Control*, promoted *Observability*). Ablation becomes the capstone. |
| L03 Repository as system of record | If knowledge isn't in the repo, the agent can't see it; "draw a map"; ACID-style agent state | Module 3 | **Kept**. Hidden-rule task (t04) tests it directly. |
| L04 One giant instruction file fails | Short entry file + topic docs (progressive disclosure) | Module 4 | **Kept**, plus measured: giant file vs. split. Added token budgets and compaction. |
| L05 Long tasks lose continuity | Context windows end; persist state in a progress file | Module 9 | **Kept**; merged with L06 and L12 into one "long-running work" module. |
| L06 Initialization needs its own phase | Separate "make the env work" from "build features"; `init.sh` | Module 9 | **Kept** as the init phase. |
| L07 Overreach and under-finish | Attention is finite; WIP=1 | Module 7 | **Kept**, enforced in the harness (a second `in-progress` feature is refused). Added diff-scope guards and plan-then-act. |
| L08 Feature lists are harness primitives | Feature state machine; `feature_list.json` | Module 6 | **Kept**, but the harness re-runs each feature's `verify` command instead of trusting the model's status. |
| L09 Agents declare victory too early | 3-layer termination check; Definition of Done | Module 6 (stop gate) | **Kept**, implemented as a gate inside the loop. |
| L10 End-to-end testing changes results | Unit-green ≠ done; validation hierarchy | Module 8 | **Kept**. Added an architecture/layer linter so F2 can be checked. |
| L11 Observability inside the harness | Agent needs runtime visibility | Module 11 | **Expanded**: added *harness* observability for the engineer (JSONL traces, viewer, replay, failure labeling). |
| L12 Every session leaves a clean state | Entropy grows by default; session exit checklist | Module 9 | **Kept** as an enforced clean-exit check. |
| L13 Loop engineering | /goal, six loop primitives (automations, worktrees, skills, connectors, sub-agents, external state); generator/evaluator separation | Module 12 | **Kept** the generator/evaluator split as the core. Tool-specific scheduling (`/loop`, Routines, Codex automations) is optional. |
| L14 Graph engineering | Prompt → context → loop → graph; nodes, edges, state, routing | Module 13 | **Kept**; build a planner → implementer → verifier graph yourself. |

## Projects

| Original project | Idea | My equivalent |
|---|---|---|
| P01 Baseline vs minimal harness | Same app, `starter/` (vague prompt) vs `solution/` (AGENTS.md, feature_list.json, init.sh, progress) | Module 1 baseline vs. Module 3+ flags, same task suite |
| P02 Agent-readable workspace | Structure the repo for agents | Module 3 (AGENTS.md + docs/) |
| P03 Multi-session continuity | Progress across sessions | Module 9, task t07 |
| P04 Incremental indexing | Feature work inside an evolving app | Tasks t03–t06 on the task repo |
| P05 Grounded QA verification | Verify outputs against sources | Modules 8 + 12 (e2e + evaluator) |
| P06 Runtime observability | Agent debugs using logs | Module 11 (`read_logs` tool) |
| P07 First loop | Build a loop | Module 12 |
| P08 First graph | Build a graph | Module 13 |

The original practice target is an **Electron knowledge-base app**. I replaced it with a **small CLI repo with fast tests and hidden acceptance tests**: faster runs, less noise, and failures that are easier to attribute.

## Topics I added (not covered, or only lightly, in the original)

1. **Writing the agent loop itself** (Module 1): messages, tool calls, stop reasons, provider adapter.
2. **Tool design** (Module 2): schemas, edit-vs-write, actionable errors, output truncation, timeouts.
3. **Context budgeting and compaction** (Module 4): token accounting, summarization, pruning old tool output, prompt caching.
4. **Control and safety** (Module 10): budgets, loop detection, permissions, sandboxing, prompt-injection tests, secret redaction.
5. **Evaluation methodology** (Module 0 + 14): hidden tests, `false_done` metric, multiple seeds, cost per success, two-model comparison.
6. **Harness observability for the engineer** (Module 11): traces, replay, failure attribution.
7. **Comparing your harness with real tools** (Module 13): Claude Code/Codex in *isolated* copies, to avoid the parent-`CLAUDE.md` leak that blurs comparisons.

## Pitfall noted from this repo

If you run the original P01 `starter/` experiment inside this repo with Claude Code, it automatically loads the repo-root `CLAUDE.md` (which describes the architecture) and can read `../solution/`. The "weak harness" run isn't really weak. **Always copy the starter out of the repo before using it as a baseline.**

## Useful files to reread

- `docs/en/lectures/lecture-01-why-capable-agents-still-fail/code/failure-pattern-demo.ts`: a toy simulation of F1–F4.
- `docs/en/lectures/lecture-02-what-a-harness-actually-is/index.md`: the 5-subsystem model and the ablation idea.
- `docs/en/lectures/lecture-13-loop-engineering/index.md`: loop primitives, generator/evaluator.
- `projects/project-01/solution/{AGENTS.md, feature_list.json, init.sh, claude-progress.md}`: concrete harness-file examples to adapt for Module 3/6/9.
- `docs/en/resources/`: templates and references.
