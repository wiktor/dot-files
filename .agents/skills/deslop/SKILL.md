---
name: deslop
description: Remove AI-generated code slop, redundant abstractions, and low-value comments or tests while preserving behavior.
---

# Remove AI Code Slop

Inspect the current changes (branch diff against `main`/base, falling back to staged/unstaged changes or files modified in the session) and remove AI-generated slop.

## Focus Areas

- **Comments**: Delete comments that restate what the code does or narrate the diff. If a comment explains a confusing name or structure, rename or restructure the code so the comment can be deleted. Keep comments explaining non-obvious *why* constraints, external quirks, tool directives, and required public docs.
- **Over-defensive code**: Remove redundant null checks, fallbacks, or `try/catch` blocks inside trusted, already-typed internal code paths. Keep validation at real trust boundaries (user input, external I/O, auth, data safety).
- **Type escapes**: Remove casts to `any` / `unknown` or lint suppressions added only to bypass type errors.
- **Reinvented wheels**: Replace newly written helpers, ad-hoc string literals, or manual logic when an existing codebase utility, enum/type, or standard library primitive already covers it.
- **Control flow & clarity**: Flatten deeply nested code with early returns. Replace nested ternaries with clear `if/else` or `switch` statements.
- **Chat-context artifacts**: Fix identifiers (`newHandler`, `tempResult`), dead code, or compatibility shims that only existed as stepping stones during the session.
- **Low-value tests**: Remove or merge tests that only assert what their own mocks return, duplicate existing coverage, or require test-only production exports.

## Guardrails

- **Preserve behavior**: Never change what the code does—only how it is expressed, unless fixing an obvious bug.
- **Clarity over brevity**: Prefer explicit, readable code over dense one-liners or clever compression. Do not remove helpful abstractions just to reduce line count.
- **Stay in scope**: Only edit files within the active diff/scope and their direct import/export seams.
- **Verify**: Run relevant typecheck, lint, and tests after edits. Never weaken or delete a failing test to make checks pass.
- **Concise output**: Keep the final summary to 1–3 sentences.
