# Review Pipeline Rules

When implementing a new feature or non-trivial behavior change:
- Before edits: delegate to `requirements-clarifier`, `impact-scout`, `design-reviewer`.
- After edits: delegate to `diff-correctness-reviewer`, `test-reviewer`, `risk-reviewer`, `cleanup-reviewer`.

When changes touch concurrency (threads, async, locks, atomics, shared state):
- After edits: delegate to `concurrency-reviewer`.

When changes touch manual memory or resource lifetime (unsafe, FFI, raw pointers, explicit free/close):
- After edits: delegate to `lifetime-reviewer`.

When any reviewer output includes assumptions, follow-up checks, probes, or a `VERIFY:` tag:
- After edits: delegate to `verification-first` before final response.
  - If a reviewer already ran a probe and provided evidence, and no open `VERIFY:` items remain, you may skip the extra `verification-first` pass.

If the change is trivial (docs, comments, formatting only), skip the pipeline.

When a follow-up fix changes the approach or expands the diff:
- Pause and summarize why the diff grew and the new scope.
- Ask for confirmation before proceeding.
- Example prompt:
  "This fix now needs to touch X and Y and adds ~Z lines beyond the original plan.
  Do you want me to proceed with the expanded approach, or keep it minimal?"
- After edits: delegate to `cleanup-reviewer`.
