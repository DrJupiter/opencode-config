# Verification-First Rules (opencode)

Core behavior:
- For feasibility or environment-dependent claims, run 1-3 minimal probes before asserting possible/impossible.
- Prefer safe, read-only, or dry-run checks; avoid destructive commands unless explicitly requested.
- If blocked by sandbox/permissions/network, say so and provide a user-run command or ask for approval.
- Separate verified facts from assumptions; include evidence (command + key output) for verified claims.
- When uncertain, delegate a quick probe plan to the `verification-first` subagent.

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
