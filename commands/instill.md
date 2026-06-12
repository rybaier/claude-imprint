Capture a hard rule and instill your rules into the current project's CLAUDE.md.

Rules are different from preferences. Preferences (in `profile.md`) are soft, observed
tendencies. Rules are hard, decided constraints — the always/nevers you want enforced.
`/instill` is the deliberate counterpart to `/imprint`: you state a rule, it files it in
your canonical ruleset and publishes it into a project so it travels with the repo.

The canonical ruleset lives at `~/.claude/working-memory/rules.md` (private, auto-loaded
every session). A project's `CLAUDE.md` is where rules get published so collaborators, CI,
and your other machines see them too.

## Usage

- `/instill <rule text>` — capture the stated rule into `rules.md`, then inject.
- `/instill` (no argument) — skip capture; inject existing rules into this project.

## Process

1. **Pre-check** — Read `~/.claude/working-memory/rules.md`. If it does not exist, tell the
   user to run `./install.sh` or `./update.sh` from claude-imprint first, and stop.

2. **Capture** (only if the user stated a rule with the command or in the conversation):
   - Reword it into a concise, actionable rule (1-2 lines) — specific enough to change
     behavior, phrased as an always/never constraint.
   - Pick the matching section in `rules.md` (`## Process`, `## Code Changes`,
     `## Communication`, or propose a new section if none fits).
   - If a near-duplicate rule already exists, propose updating it rather than adding a
     second. Otherwise propose the addition.
   - Present the proposed change in this format and **wait for approval**:
     **File**: `~/.claude/working-memory/rules.md`
     **Section**: <section>
     **Entry**: <the rule>
   - On approval, use the Edit tool to add/update the entry. Increment
     `<!-- stats: rules=N -->` (set `since` to today if it is a placeholder) and set
     `<!-- last-instill: -->` to today.

3. **Resolve the target project** — Run `git rev-parse --show-toplevel` to find the repo
   root; if not in a git repo, use the current working directory. Confirm the target with
   the user: "Instill rules into `<path>/CLAUDE.md`?"

4. **Choose which rules to inject** — Default to all entries in `rules.md`. Offer to inject
   a subset if the user prefers. Never inject the placeholder `<!-- e.g., ... -->` lines.

5. **Apply to the project CLAUDE.md** — Manage a single marked block, matching the marker
   convention used by `claude-md-snippet.md`:

   ```
   <!-- BEGIN claude-imprint rules -->
   ## Rules
   <the selected rules, grouped by their sections>
   <!-- END claude-imprint rules -->
   ```

   - **File absent**: create `CLAUDE.md` containing the block.
   - **Block already present**: replace its contents (idempotent — re-running never
     duplicates).
   - **Rules present outside the block** (e.g. hand-written earlier): point them out and
     offer to consolidate them into the managed block, removing the loose copies.
   - Always show the proposed file content or a diff, and **wait for approval** before
     writing. Never write to a project CLAUDE.md automatically.

6. **Close out** — Confirm what was captured (if anything) and which rules were instilled
   into which file. Remind the user the project CLAUDE.md is git-tracked, so committing it
   shares the rules with the repo.

## Notes

- This command only ever writes to two places: `~/.claude/working-memory/rules.md` (capture)
  and the target project's `CLAUDE.md` (inject). It never touches other files.
- Capture is private; injection is a publish step. Both require explicit approval.
