# Achieve Loop Protocol

Read the goal and success criteria from the state.yaml file in this session directory.

## Each Iteration

### 1. READ STATE
- Check `state.yaml` in this session directory for goal, criteria, phase
- Check iteration count in `loop.md` frontmatter

### 2. ROUTE BY PHASE

---

**A. phase=research**

Explore the codebase to understand what exists and gather context for implementation.

**Actions:**
1. Search for files related to the goal using Glob and Grep
2. Read key files to understand existing patterns and architecture
3. Identify code that can be reused or extended
4. Note dependencies and components that will be affected

**Document findings in `research.md`:**
```markdown
# Research Findings

## Relevant Files
- path/to/file.ts - description of what it does

## Existing Patterns
- Pattern name: how it's used

## Dependencies
- What this goal depends on

## Architecture Notes
- Key observations about the codebase structure

## TLDR
- Bullet summary of key findings
```

**After research:** Update `current_phase` to `plan` in state.yaml

---

**B. phase=plan**

Create a specification and implementation plan based on research findings.

**Create `spec.yaml`:**
```yaml
# Specification
goal: "<goal from state.yaml>"
requirements:
  functional:
    - FR1: Description
  non_functional:
    - NFR1: Description
success_criteria:
  # Copy from state.yaml
assumptions:
  - List assumptions made
dependencies:
  - External dependencies
open_questions:
  - Any unclear requirements (ask user if critical)
```

**Create `plan.yaml`:**
```yaml
# Implementation Plan
phases:
  - name: "Phase 1 name"
    tasks:
      - [ ] Task 1 description
      - [ ] Task 2 description
  - name: "Phase 2 name"
    tasks:
      - [ ] Task 3 description

architecture_decisions:
  - Decision: Rationale

files_to_modify:
  - path/to/file.ts - what changes

files_to_create:
  - path/to/new.ts - purpose

testing_approach:
  - How to verify the implementation
```

**If requirements are unclear:** Ask clarifying questions using AskUserQuestion before finalizing the plan.

**After planning:** Update `current_phase` to `build` in state.yaml

---

**C. phase=build**

Execute the implementation following the plan.

**Actions:**
1. Read `plan.yaml` to get current tasks
2. Implement tasks in order
3. Mark tasks complete as you go: `- [x] Task`
4. If you deviate from plan, log it:
   ```yaml
   deviations:
     - original: "What was planned"
       actual: "What was done"
       reason: "Why the change"
   ```
5. After each significant change, verify it compiles/runs

**Skill Discovery (optional):**
- Check if `.claude/skills/` exists
- If yes, read available skills and use relevant ones
- Skills can accelerate implementation for specific domains

**After build completes:** Update `current_phase` to `validate` in state.yaml

---

**D. phase=validate**

Test against each success criterion from state.yaml.

**Actions:**
1. Run tests, builds, or other verification commands
2. Check each criterion explicitly
3. Document results

**If ALL criteria pass:**
- Update status to `achieved` in state.yaml
- Output: `<promise>ACHIEVED</promise>`

**If ANY criterion fails:**
- Log attempt in state.yaml under `attempts:`
- Analyze what failed and why
- Update `current_phase` back to `build`
- Plan the fix approach

---

### 3. LOG ATTEMPTS

After each validation, add to attempts in state.yaml:
```yaml
attempts:
  - iteration: N
    result: pass/fail
    details: what worked/failed
```

### 4. COMPLETION

When ALL success criteria are met:
- Summarize what was accomplished
- Output: `<promise>ACHIEVED</promise>`

**IMPORTANT**: Only output `<promise>ACHIEVED</promise>` when ALL criteria are met.
Do NOT output partial achievement promises - keep iterating until success or max iterations.

### 5. MAX ITERATIONS

If max iterations reached without full success:
- The stop hook will automatically end the session
- Summarize progress made and criteria achieved
- List remaining criteria not met with clear explanation
- Session status will be set to `max_iterations_reached`

### 6. HANDLING DIFFICULT CRITERIA

If a criterion proves difficult after multiple attempts:
- Document varied approaches tried
- Explain the specific challenge
- **Do NOT give up** - continue iterating with new approaches
- The session only ends at max_iterations or full achievement
