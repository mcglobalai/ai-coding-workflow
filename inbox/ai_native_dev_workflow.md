# AI-Native Development Workflow

## Core Idea

Human writes decisions. AI writes everything else.

```
Human → TDD (decisions + contract + rules + examples)
           ↓
         AI → Spec → Code → Tests
           ↓
Human → Verify against TDD (not against code)
```

## Why This Works

Traditional engineering docs (PRD → TDD → Spec → Code) exist to prevent miscommunication between people. When AI replaces the middle layers, miscommunication isn't the failure mode — scope creep and silent assumptions are. 

So we optimize differently:

- **Human attention → judgment calls only.** Design decisions, I/O boundaries, behavioral rules, calibration examples. Everything that requires taste or domain knowledge.
- **AI attention → expansion work.** Full test suites, edge cases, error handling, implementation details. Everything that is derivable from the human-provided constraints.
- **Verification stays at the human layer.** If golden examples pass and acceptance criteria are met, the task is done. No need to review generated code line-by-line unless something fails.

## The TDD Document

One document per task. Seven sections, each with a clear purpose:

| Section | Who writes | Purpose |
|---|---|---|
| **Intent** | Human | Why this task exists (2-3 sentences) |
| **Decisions** | Human | Judgment calls AI must not re-decide |
| **Contract** | Human | Exact input/output types — the boundary |
| **Rules** | Human | Testable behaviors (each → one assert) |
| **Examples** | Human | Hand-verified calibration points (2-3) |
| **Done When** | Human | Acceptance checklist |
| **Not This** | Human | Scope boundaries to prevent AI expansion |

Bottom of document: **AI Spec Generation Instructions** — tells AI how to read each section and what to generate from it.

## How It Fits Into Task Management

```
1. Human identifies task → writes TDD (30-60 min)
2. TDD review (self or peer) → status: Ready
3. AI generates Spec + Code + Tests from TDD
4. Human runs acceptance criteria checks
5. Pass → Done. Fail → human inspects AI artifacts, updates TDD if needed, re-run.
```

Each task is tracked by its TDD file. Status lives in the TDD header. A master `tasks.md` lists all active TDDs with status and dependencies.

## When to NOT Use This

- **Exploratory research** (don't know the contract yet) → use conversation/notebook first, write TDD after decisions crystallize
- **Trivial changes** (< 30 min implementation) → just do it
- **Design still in flux** → the TDD's Decisions section is the forcing function; if you can't write decisions, you're not ready for a TDD
