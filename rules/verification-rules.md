# Verification-First Rules 

Core behavior:
- For feasibility or environment-dependent claims, run 1-3 minimal probes before asserting possible/impossible.
- Prefer safe, read-only, or dry-run checks; avoid destructive commands unless explicitly requested.
- If blocked by sandbox/permissions/network, say so and provide a user-run command or ask for approval.
- Separate verified facts from assumptions; include evidence (command + key output) for verified claims.
- When uncertain, delegate a quick probe plan to the `verification-first` subagent.
- Reviewers may run 1-2 read-only probes if it helps; include evidence and a suggested cache line. If a probe isn't possible, use `VERIFY:`.
- If you rely on an undocumented or uncertain behavior while writing code, either run verification-first or mark it explicitly as an assumption and propose a probe.

Trigger conditions (must run verification-first unless explicitly waived by user):
- Review output includes "Assumptions", "Follow-up checks", "probe", or a `VERIFY:` tag.
- A reviewer suggests an experiment or correction that depends on runtime/environment/tooling.
- You make a claim about tool availability, version, CLI flags, or system behavior not verified in this session.
- You need evidence to decide between two implementation options.

Tag convention:
- Use `VERIFY:` to mark any assumption or experiment that should trigger verification-first.

Verification cache:
- Location: project_root/.opencode/verification-cache/<scope>.log
- project_root: nearest ancestor with .git or a common project marker (package.json, pyproject.toml, go.mod, Cargo.toml). If none or not writable, use ~/.local/share/opencode/verification-cache/<project-slug>/.
- Use /tmp/opencode-verify/<project-slug>/ only for explicit one-off or ephemeral tasks.

Log format (one line per entry, ASCII only):
YYYY-MM-DD|scope|tags|claim|evidence|confidence|expires

Guidelines:
- Default TTL 30 days; network 7 days; tool availability 90 days (override if needed).
- Keep lines <= 240 chars; if evidence is long, write to .../evidence/<id>.txt and reference the path.
- Query with `rg` and load only matching lines; ignore expired entries.
- If a probe is inconclusive, say so and propose the next probe.
