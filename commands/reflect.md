Review working memory files and provide a health check on the accumulated learnings.

## Process

1. **Read all working memory files**:
   - `~/.claude/working-memory/profile.md`
   - `~/.claude/working-memory/collaboration-patterns.md`
   - `~/.claude/working-memory/boundaries.md`
   - `~/.claude/working-memory/rules.md` (if it exists)
   - `~/.claude/working-memory/tools.md` (if it exists)
   - `~/.claude/working-memory/anti-patterns.md` (if it exists)
   - `~/.claude/working-memory/team/team-patterns.md` (if it exists)

2. **Generate a summary** of what Claude has learned about the user. Write this as a
   short narrative (not just repeating the bullets back). Frame it as: "Here's what I've
   learned about how you work." This should feel like a useful mirror, not a recitation.

3. **Check freshness** using each file's metadata comments:
   - `<!-- last-reviewed: YYYY-MM-DD -->` — flag if older than 7 days or still a
     `YYYY-MM-DD` placeholder (meaning never reviewed)
   - `<!-- last-imprint: YYYY-MM-DD -->` — note when the last learning was captured
   - `<!-- imprint-count-since-reflect: N -->` in `~/.claude/CLAUDE.md` — flag if N >= 5
     (meaning 5+ /imprint runs since last /reflect)

4. **Detect issues** across all files:
   - **Stale entries**: Patterns that haven't been reinforced recently or that contradict
     recent session behavior
   - **Contradictions**: Entries that conflict with each other across or within files
   - **Redundancy**: Multiple entries that say the same thing differently — propose merging
     into one clean entry
   - **Vague entries**: Entries too generic to change behavior (e.g., "write clean code") —
     propose sharpening or removing
   - **Gaps**: Sections with few or no entries that could benefit from attention
   - **Rule promotion**: Preferences in `profile.md` / `collaboration-patterns.md` that have
     hardened into always/never constraints — reinforced repeatedly, or corrected 2+ times in
     `session-log.md`. Propose promoting these to `rules.md` (use the `promote` action below).
     A hardened preference is a rule the user keeps having to restate; moving it to `rules.md`
     lets `/instill` carry it into projects.
   - **Rule health** (`rules.md`, if it exists): duplicate rules, rules that contradict each
     other or a preference, and rules violated in the current session.

5. **Effectiveness scoring** (if in an active session with history):

   a. **Current session scan**: Review the current session's conversation history.
      For each entry across all memory files, classify:
      - **Followed**: Claude's behavior was consistent with the entry
      - **Violated**: Claude did something the entry should have prevented
      - **N/A**: Entry wasn't relevant this session
      Skip files where all entries are N/A.

   b. **Calculate scores**: For each file with relevant entries, compute:
      `Score = followed / (followed + violated)` as a percentage.

   c. **Violation details**: For each violated entry, explain:
      - Which entry was violated and in which file
      - What happened in the session that contradicted it
      - Why the current wording didn't prevent the mistake
      - A proposed rewrite that would be more effective

   d. **Longitudinal trend** (if `~/.claude/working-memory/session-log.md` exists
      with 5+ entries):
      - Scan session-log.md for past entries that mention corrections
      - Cross-reference with current memory entries
      - Flag entries that have been corrected 2+ times in the last 10 sessions —
        these are candidates for rewriting (the current wording isn't working)

   e. **Effectiveness ratio** (if stats comments exist):
      Compare correction counts against violations found. If a file has high
      corrections but entries keep getting violated, note that the file may need
      a rewrite pass rather than incremental additions. If corrections are low
      but many sessions have passed, suggest the user may be under-using /imprint.

   Present scores in the Health section as a table:
   | File | Entries | Followed | Violated | N/A | Score |
   |------|---------|----------|----------|-----|-------|

   Frame violations as questions, not assertions — the user may have intentionally
   deviated from a pattern: "Did you mean to deviate from X, or should we sharpen
   the entry?"

6. **Session log maintenance**: If `~/.claude/working-memory/session-log.md` exists,
   check the `<!-- entry-count: N -->` comment:
   - If N > 100, propose archiving entries older than 90 days to
     `~/.claude/working-memory/session-log-archive-{YYYY}.md` (by year of entry date)
   - If N > 200, flag: "Session log needs immediate trimming — over 200 entries"
   - Report entry count in the Health section

7. **Present findings** in this format:

   ### Summary
   (Narrative of what Claude has learned)

   ### Health
   - **Entries**: X total across Y files
   - **Corrections captured**: N total (breakdown per file from `<!-- stats: -->` comments)
   - **Tracking since**: earliest `since` date across files, or "not yet tracked"
   - **Last reviewed**: (date per file, or "never")
   - **Last imprinted**: (date per file from `<!-- last-imprint: -->`)
   - **Imprint runs since last reflect**: N (from `<!-- imprint-count-since-reflect: -->`)
   - **Session log entries**: N (from `<!-- entry-count: -->` in session-log.md, or "no log")
   - **Freshness**: (ok / stale / needs attention)

   ### Proposed Changes
   For each proposed change:
   - **Action**: merge / remove / sharpen / add / promote
   - **File**: which file
   - **Current**: the existing entry (if applicable)
   - **Proposed**: the new version
   - **Reason**: why this change improves the memory

8. **Wait for approval** before making any changes.

9. After approval, apply the changes and update the `<!-- last-reviewed: YYYY-MM-DD -->`
   comment at the top of each reviewed file to today's date.

9b. Reset `<!-- imprint-count-since-reflect: N -->` to 0 in `~/.claude/CLAUDE.md`.
