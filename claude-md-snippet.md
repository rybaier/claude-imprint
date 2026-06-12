<!-- BEGIN claude-imprint -->
## Working Memory

Claude maintains a working memory of the user's development style and collaboration patterns.
These files are located in `~/.claude/working-memory/` and should be consulted at the
start of meaningful work sessions.

### Required Reading
At the start of sessions involving significant work (not quick one-off questions), read:
- `~/.claude/working-memory/profile.md` — Working style and preferences
- `~/.claude/working-memory/collaboration-patterns.md` — What works in pairing with this user
- `~/.claude/working-memory/boundaries.md` — Work vs personal separation rules
- `~/.claude/working-memory/rules.md` — Hard rules: always/never constraints to enforce (if it exists)
- `~/.claude/working-memory/tools.md` — Preferred tools and environment (if it exists)
- `~/.claude/working-memory/anti-patterns.md` — Mistakes to avoid (if it exists)
- `~/.claude/working-memory/team/team-patterns.md` — Team conventions (if it exists)
- `~/.claude/working-memory/session-log.md` — Recent sessions (skim the last 5-10 entries for context on recent work — do not read the entire file)

### Proactive Update Protocol
When any of the following happen during a session, suggest updating working memory
(ask before writing — never silently update):
- The user corrects an assumption or approach
- A new workflow preference emerges
- A collaboration pattern proves effective or ineffective
- The user explicitly asks to remember something

Updates go to the appropriate file:
- Style/preference -> `profile.md`
- Collaboration insight -> `collaboration-patterns.md`
- Separation rule -> `boundaries.md`
- Hard rule / always-never constraint -> `rules.md` via `/instill`
- Tool/environment preference -> `tools.md`
- Mistake/anti-pattern -> `anti-patterns.md`
- Team convention -> `team/team-patterns.md`

### Session Nudging
At the end of meaningful sessions (not quick one-off questions), evaluate whether to
suggest `/imprint` or `/reflect`:

**Suggest `/imprint`** when any of these occurred during the session:
- The user corrected an assumption or approach
- A new workflow preference emerged
- A collaboration pattern proved effective or ineffective
- A mistake was made that should be avoided in the future
- A tool preference or environment detail was established

**Suggest `/reflect`** when:
- Any working memory file has a `<!-- last-reviewed: -->` date older than 7 days
  (or still a `YYYY-MM-DD` placeholder, meaning it has never been reviewed)
- The `<!-- imprint-count-since-reflect: N -->` comment shows N >= 5
  (meaning 5+ /imprint runs since the last /reflect)
- Claude violated a working memory entry during the current session
  (a pattern was in memory but Claude didn't follow it)
- The user asks about what Claude has learned or how memory works

**Suggest `/distill`** when:
- The user mentions working on another machine or syncing preferences
- claude-imprint is freshly installed on a new machine
- It has been 30+ days since the last distill (check `~/.claude/imprinted-memories/machines/{hostname}/.snapshot-date`)
- The user asks about differences between how Claude works on different machines

**Suggest `/instill`** when:
- The user states a hard, universal rule — an always/never constraint they want enforced
  (distinct from a soft preference, which goes to `/imprint`)
- The user is doing significant work in a repo whose `CLAUDE.md` does not yet contain their
  rules and would benefit from having them present and shared with the repo

Never run any command silently. Always ask the user first.

### First-Session Onboarding
If `~/.claude/working-memory/profile.md` has no `<!-- onboarding-complete: -->` comment
and contains only template placeholders (lines starting with `<!-- e.g.,`), suggest:
"Want to run `/teach` to set up your preferences quickly? Or just start working and
use `/imprint` to build memory organically." Only suggest this once per session.

### Separation Rules
- Working memory captures HOW the user works — patterns, preferences, style
- `rules.md` is authoritative for hard constraints (always/never). Preferences are soft and
  observed (`profile.md`); rules are decided and enforced. When a preference hardens into a
  rule the user keeps restating, `/reflect` can promote it and `/instill` carries it into projects
- Team conventions (branch naming, PR requirements, deploy process) go in `team/team-patterns.md`
- Project-specific details (architecture, conventions, gotchas) are handled by Claude's
  built-in auto memory — /imprint focuses on transferable patterns only
- When unsure whether something is a global pattern vs project-specific, ask the user

### Relationship with Auto Memory
Working memory files are authoritative for the categories imprint covers (style, collaboration,
boundaries, tools, anti-patterns, team conventions). Claude's built-in auto memory should not
duplicate entries that already exist in working memory files. Auto memory handles project-specific
context; working memory handles developer identity.

<!-- imprint-count-since-reflect: 0 -->
<!-- END claude-imprint -->
