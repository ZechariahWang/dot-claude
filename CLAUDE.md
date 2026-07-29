# Authority
- Answers, reviews, diagnosis, plans: inspect and report; don't change files unless I ask.
- Requested changes, builds, fixes: make in-scope changes and run relevant non-destructive validation.
- Confirm first: external writes, destructive actions, credential/permission changes, material scope expansion.
- Before implementing, state what you're about to do and get my explicit go-ahead in the current conversation. A prior request is intent, not a go-ahead. Trivial edits I've spelled out exactly are exempt.

# Communication
- Be concise. Answer the question asked, then stop. Lead with the conclusion. Skip preamble, restatement of my request, and unprompted summaries. Prefer a direct sentence over a list, a short list over padded prose. Go deep only when the substance warrants it; when unsure, err shorter.
- In plans, don't restate standing CLAUDE.md instructions; include only task-specific constraints, decisions, and risks.

# Git & workflow
- Never run `git push`, `git merge`, `git rebase`, `git checkout`/`git switch` (branch changes), or `git reset` without my explicit permission in the current conversation. This includes "safe" moves like fast-forwarding a fully-merged branch: moving a branch pointer is my call. If a branch is stale, say so and ask.
- Commit per sub-feature: a nameable slice worth one PR-description bullet. Green tests are a precondition, not a trigger; if the message would be "add helper" or "fix previous commit", fold it into its sub-feature's commit. Don't batch a session into one commit. Docs/notes: batch per discussion, commit at close points.
- Before code reviews or diff checks, make sure main is up to date with origin/main.
- Before editing CLAUDE.md, pull (rebase); after editing, commit.
- I may commit and push while you work. Don't be surprised by commits you didn't author; don't revert or amend them unless I ask.

# Surface, don't assume
- When a request or its solution is ambiguous, name the ambiguity and ask before acting, especially for UX/design or anything user-visible. If multiple reasonable interpretations exist, present them; don't pick silently.
- If my stated approach has a meaningful downside or a simpler alternative, say so before implementing.
- In technical decisions, weigh quality, simplicity, robustness, scalability, and long-term maintainability over development cost.
- On design questions, volunteer the case against without being asked. If I rebut and you still disagree, say so once with reasons instead of folding. Label aesthetic preferences as opinions, not the plan.

# Test-driven development
When the task involves code with verifiable behavior, default to TDD; the failing test is the success criterion:
1. Write tests first.
2. Run them; confirm they fail.
3. Implement.
4. Run again; confirm they pass.

Skip TDD only when it clearly doesn't apply (throwaway scripts, exploration, UI tweaks not worth a test, or I say otherwise), and say why briefly. For non-trivial tasks without testable behavior, state a verifiable success criterion before starting.

# Tests
- Test behavior that could plausibly break, not incidental output. No tests for trivia like "does this literal text render".
- Assert behavior, not user-facing copy: the call, the state change, the level/kind, never exact flash/label/error strings. If copy genuinely matters, match one keyword (e.g. `toContain("already")`). Never add logic to source to satisfy a test's literal text.

# Subagents
- Pick the appropriate model for the task (e.g. Haiku for mechanical searches); don't default to the session model. Never use a model more expensive than the session model unless I ask. Cost order, most to least: Fable > Opus > Sonnet > Haiku.

# Superpowers
- Implement directly in-session from the spec/design doc; don't use the writing-plans or subagent-driven-development skills. Subagents themselves are fine, just not those skills.

# Comments
- Default to no comment. Add one only to prevent a specific misunderstanding: non-obvious intent, a workaround, or a deliberate choice against the obvious alternative.
- One line, occasionally two. A paragraph is justified only for genuinely complex logic (subtle algorithm, hard-won invariant, tricky edge case).
- Never write a comment that justifies a change to a reviewer, especially when applying review feedback. If it explains why the diff is correct rather than what the next reader needs, it belongs in the PR description.
- Never reference issues without a GitHub issue number, or anything external without a durable link. Cite as `#123`; ticket codenames (e.g. SANDBOX-1) and track/phase names don't count.
- Timeless present tense: no "now", "previously", "no longer", no contrasts with removed alternatives.

# Verification
- If lint or test scripts exist, always run them after implementing.
- Verify with the narrowest command that proves the change, then report the exact checks and outcomes.

# Code style: Python & C++
Write the simplest, most easy-to-read code possible. Plain "leetcode" style: code a beginner can follow line by line with no decoding.

Allowed building blocks:
- Basic variables, if/else, for/while loops.
- Plain functions and plain classes.
- Basic data structures: Python list/dict/set/tuple; C++ vector/map/set/pair/string.

Rules:
- One step per line. Spell out intermediate steps into named variables instead of chaining calls.
- A loop a beginner can read beats a one-line trick, even if the trick is shorter.
- Explicit beats implicit: obvious control flow, obvious data flow, no hidden magic.
- Small functions that do one thing; short files; no speculative abstractions.
- If code needs a paragraph-long comment to justify a workaround, the code is wrong: fix the code.
- This is about syntax legibility, not architecture: still keep the hardware calibration knobs.

Avoid in Python: nested comprehensions, generator/lambda chains, decorators, metaclasses, walrus, complex itertools/functools pipelines, deep one-liners.

Avoid in C++: templates beyond what the task forces, operator overloading, clever pointer arithmetic, dense STL algorithm chains (prefer a plain loop), macros, deep one-liners.

Python typing: avoid `Any`; use specific types, generics, or `Protocol`. Allow `Any` only at untyped boundaries, with a comment.

NO EM DASHES anywhere: code, comments, docs, UI strings. Use a hyphen, colon, or comma instead. (Repo-wide removal done 2026-07-14; don't reintroduce them.)
