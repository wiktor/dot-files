---
name: deslop
description: Remove AI-generated slop, redundant abstractions, and low-value comments, tests, or boilerplate while preserving behavior and intent.
---

# Remove AI Slop

Inspect the current changes (branch diff against `main`/base, falling back to staged/unstaged changes or files modified in the session) and remove AI-generated slop.

## Focus Areas

- **Comments & prose**: Delete comments that restate the implementation or narrate the diff. If a comment explains a confusing name or structure, rename or restructure so the comment can be deleted. Strip AI filler and repetitive boilerplate from docs and configs. Keep comments explaining non-obvious *why* constraints, external quirks, tool directives, and required public docs.
- **Over-defensive checks**: Remove redundant checks, fallbacks, or error-handling wrappers inside trusted, already-validated internal paths. Keep validation at real trust boundaries (user input, external I/O, auth, data safety).
- **Type & check escapes**: Remove unsafe type casts, untyped/wildcard annotations, or compiler/linter suppression directives added only to silence errors rather than fixing the root cause.
- **Reinvented wheels**: Replace newly written helpers, raw literals, or manual logic when an existing project utility, shared constant/type, or standard library primitive already covers it.
- **Structure & control flow**: Flatten deep nesting with early returns or guard clauses. Replace nested ternaries or dense expressions with clear conditional branches.
- **Chat-context artifacts**: Remove stepping-stone names, dead code, unused configuration, or temporary shims that only make sense in the context of the conversation.
- **Low-value tests**: Remove or merge tests that only assert what their own mocks/stubs return, duplicate existing coverage, or require test-only production hooks.

## Guardrails

- **Preserve behavior & intent**: Never change what the system does—only how it is expressed, unless fixing an obvious bug.
- **Clarity over brevity**: Prefer explicit, readable structure over dense one-liners or clever compression. Do not remove helpful abstractions just to reduce line count.
- **Match local idioms**: Follow the conventions, patterns, and style of the surrounding file and repository.
- **Stay in scope**: Only edit files within the active diff/scope and their direct interfaces.
- **Verify**: Run relevant project checks (build, typecheck, lint, tests) after edits. Never weaken or delete a failing test to make checks pass.
- **Concise output**: Keep the final summary to 1–3 sentences.
