# DSA Drillbook Expansion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [x]`) syntax for tracking.

**Goal:** Expand `python_dsa_patterns_explained.md` into a fuller interview drillbook across all existing DSA topics.

**Architecture:** This is a single-document content expansion. The existing topic order and code samples remain the source of truth; new drillbook sections are inserted near the concepts they explain.

**Tech Stack:** Markdown, PowerShell, ripgrep, git.

---

### Task 1: Add Drillbook Sections To Core Topic Chapters

**Files:**
- Modify: `python_dsa_patterns_explained.md`

- [x] **Step 1: Inspect current headings**

Run: `rg -n "^## |^### " python_dsa_patterns_explained.md`

Expected: A list of existing topic and subsection headings, including Topics 1 through 13.

- [x] **Step 2: Add interview drill sections for each main topic**

For each topic from Arrays through Big-O, add concise Markdown subsections where they are missing:

```markdown
### Interview Drillbook

- **Pattern trigger:** Use this when the problem asks for repeated lookup, ordered scanning, reachability, top-k selection, or optimal decisions over repeated subproblems.
- **Core invariant:** State the condition that remains true after every loop iteration or recursive call.
- **What to say first:** Start with brute force, name the bottleneck, then explain which data structure or traversal removes it.
- **Edge cases to test:** Empty input, one element, duplicates, negative values, disconnected components, and boundary indexes where relevant.
- **Common trap:** Coding a memorized template without explaining why the template matches the problem.
- **Follow-up direction:** Be ready to discuss space optimization, returning the actual path/subsequence, streaming input, or larger constraints.
```

Adapt the bullets to the topic. Keep each bullet practical and interview-facing.

- [x] **Step 3: Preserve existing code blocks**

Run: `rg -n "^```" python_dsa_patterns_explained.md`

Expected: Code block fence count remains even.

### Task 2: Strengthen Compact Pattern Reference And Study Method

**Files:**
- Modify: `python_dsa_patterns_explained.md`

- [x] **Step 1: Expand compact pattern entries**

Add short interview-ready bullets to each compact pattern section:

```markdown
### Interview Reflex

- Start with the brute-force decision the algorithm is improving.
- Keep saying the invariant that proves the current state is correct.
- Test the smallest input, a duplicate-heavy input, and a boundary input.
```

Use the existing compact section as review material rather than adding long prose.

- [x] **Step 2: Expand final study method**

Add a short drill routine for one-day prep and weekly prep:

```markdown
### Drill Routine

- 10 minutes: identify pattern triggers.
- 25 minutes: solve one problem out loud.
- 10 minutes: rewrite the invariant and edge cases.
```

### Task 3: Validate Markdown And Review Diff

**Files:**
- Modify: `python_dsa_patterns_explained.md`

- [x] **Step 1: Check heading shape**

Run: `rg -n "^## Topic|^### Interview Drillbook|^### Interview Reflex|^### Drill Routine" python_dsa_patterns_explained.md`

Expected: Every major topic has an `Interview Drillbook` section; compact reference areas have `Interview Reflex` where useful.

- [x] **Step 2: Check code block fences**

Run: `powershell -NoProfile -Command "(Select-String -Path python_dsa_patterns_explained.md -Pattern '^```').Count"`

Expected: The count is an even number.

- [x] **Step 3: Review changed lines**

Run: `git diff -- python_dsa_patterns_explained.md`

Expected: The diff shows Markdown additions only, with no accidental deletion of existing solutions.

- [x] **Step 4: Commit implementation**

Run:

```bash
git add python_dsa_patterns_explained.md docs/superpowers/plans/2026-06-17-dsa-drillbook-expansion.md
git commit -m "Expand Python DSA drillbook notes"
```

Expected: A commit containing the plan and the expanded Markdown guide.
