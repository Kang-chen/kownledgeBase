---
name: memory-invocation
description: Load and apply the personal knowledge base Agent Memory. Use when Codex needs prior project context, reusable workflows, historical decisions, experiment lessons, archived inbox knowledge, or when the user says memory, 记忆, 历史经验, 之前的上下文, 复用, or asks how past work should guide the current task.
---

# Memory Invocation

## Purpose

Use Agent Memory as a compact retrieval layer over `knowledge/_memory/entries/`. Memory entries provide reusable context, constraints, workflows, and lessons. They are not automatically current facts.

## Workflow

1. Load `knowledge/_memory/_index.md`.
2. Extract 3-8 search keys from the user request: project, artifact names, method names, skills, agents, metrics, and failure modes.
3. Select at most 3 memory entries first. Prefer entries with matching project and actionable type:
   - `procedure`: use as workflow.
   - `decision`: preserve constraints and rationale.
   - `reference`: use as background or template.
   - `finding`: use as evidence, but check whether it still applies.
4. Read selected entry files from `knowledge/_memory/entries/`.
5. If the entry is insufficient, follow its `source` path into `knowledge/archive/YYYY-MM/` and read only the relevant section.
6. Apply memory as constraints or reusable patterns. Do not treat paths, metrics, server state, library versions, or people/status information as current without verification.
7. When new reusable learning appears, decide whether to:
   - update memory: reusable but not procedural enough for a skill;
   - create/refine a skill: stable trigger, stable steps, likely reuse;
   - leave in archive only: low reuse, one-off, or too context-specific.

## Selection Rules

Use this adapted RICE/MoSCoW filter before recommending a new skill:

| Question | Skill if yes | Memory only if |
|---|---|---|
| Trigger clarity | A future request would clearly trigger it | It needs manual interpretation |
| Procedure stability | Steps are mostly repeatable | Steps depend on project state |
| Reuse frequency | Likely monthly or across projects | Rare but worth remembering |
| Risk reduction | Prevents costly mistakes | Mostly informative |
| Effort | Can be concise and maintained | Would become a large vague manual |

Recommend no more than 3 items. Each recommendation must include one decision: `create skill`, `refine existing skill`, `memory only`, or `defer`.

## Reporting

When summarizing retrieved memory, do not list everything. Use this shape:

```markdown
## Key Reuse
1. Decision: create/refine/memory/defer
   Why: one sentence
   Use next time: one concrete trigger or command
```

Keep the user-facing report short enough that the user can answer with a simple choice.
