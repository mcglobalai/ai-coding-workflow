# CKG TDD Template v2

---

# [TASK-ID]: [Title]

**Status:** Draft | Review | Ready | In Progress | Done  
**Owner:**  
**Blocks:**

## Intent

Why this exists. 2-3 sentences max.

## Decisions

Human judgments AI must not re-decide.

1. **[what]:** [choice] — [why]

## Contract

```python
def f(input: Type) -> OutputType:
```

Input/output types with one-line field comments.

## Rules

Testable. Each maps to an assert.

1. [condition] → [behavior]

## Examples

Hand-verified. AI expands from these.

```
In:  ...
Out: ...
```

## Done When

- [ ] [criterion]

## Not This

- NOT: [scope boundary]

---

> **AI Spec Generation Instructions**
> 
> When generating an implementation spec from this TDD:
> 1. Read **Decisions** as hard constraints — never override
> 2. Read **Contract** as exact interface — do not rename or retype
> 3. Read **Rules** → expand into full test suite (edge cases, failure modes, property tests)
> 4. Read **Examples** → use as golden tests, generate 5+ additional cases covering rule combinations
> 5. If a rule is ambiguous or conflicts with another, **stop and ask** — do not assume
> 6. Output spec follows this structure: Interface (from Contract) → Full Rules (expanded) → Test Cases (expanded) → Dependencies → Error Handling
