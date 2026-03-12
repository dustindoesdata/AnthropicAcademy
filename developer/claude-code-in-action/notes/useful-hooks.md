# Useful Hooks!

**Course:** Claude Code in Action
**Section:** Hooks and the SDK
**Status:** ✅ Complete

---

## Objective

Learn two practical, high-value hooks that solve common Claude Code weaknesses — type errors and duplicate code.

---

## Key Concepts

### Hook 1: TypeScript Type Checker

**Problem:** Claude edits function signatures but doesn't always update all call sites, causing type errors downstream.

**Solution:** Post-tool use hook that runs `tsc --no-emit` after any TypeScript file is edited.

**Flow:**
1. Claude edits a TypeScript file
2. Hook fires and runs the type checker
3. Type errors are detected and fed back to Claude
4. Claude automatically fixes the broken call sites

**Adaptable to:**
- Any typed language with a type checker
- Untyped languages — substitute a test runner instead

---

### Hook 2: Duplicate Code Prevention (Query Deduplication)

**Problem:** In complex or multi-step tasks, Claude loses focus and creates new queries or functions instead of reusing existing ones.

**Solution:** A hook that monitors a critical directory (e.g., `queries/`) and launches a separate Claude instance to review changes for duplication.

**Flow:**
1. Claude edits a file in the watched directory
2. Hook detects the change
3. A secondary Claude instance is launched via the TypeScript SDK
4. Secondary Claude compares the new code against existing code in the directory
5. If a duplicate is found: exit code `2` + feedback message
6. Primary Claude receives the feedback and reuses the existing code instead

**Trade-offs:**

| Pro | Con |
|---|---|
| Cleaner codebase, less duplication | Additional time per edit |
| Self-correcting without manual review | Additional API cost |

**Recommendation:** Only watch critical, high-value directories to minimize overhead.

---

## Key Takeaways

- Hooks create automated feedback loops that catch Claude's consistent weak spots
- Post-tool use hooks are the right pattern for both of these — run checks after edits, feed results back
- Using a secondary Claude instance as a reviewer inside a hook is a powerful pattern (agentic checks)
- Be selective about which directories trigger expensive hooks

---

## Personal Notes

- The secondary Claude reviewer pattern is essentially a self-review loop — the same concept as a code review step in a CI pipeline, but AI-driven
- Type checker hook is a zero-cost safety net for any TypeScript project — should be default on
- The cost/quality tradeoff on the deduplication hook is a real business decision — applies the "business drives technology" lens: is the codebase cleanliness worth the API spend?

---

## Follow-Up Questions

- Can the secondary Claude instance in the deduplication hook use a lighter/cheaper model to reduce cost?
- How does the deduplication hook handle intentional variations of similar queries?
- Can this pattern be extended to check for duplicate components in a UI library or duplicate table definitions in a data model?
