Extract universal patterns from working memory across all your machines.

## First-Run Setup

If `~/.claude/imprinted-memories/` does not exist:

1. Explain the concept: "Distill syncs your working memory across machines. It snapshots
   this machine's memory to a private GitHub repo, compares it against snapshots from your
   other machines, and promotes patterns that show up everywhere to a shared universal set."

2. Ask: "Do you already have an `imprinted-memories` repo from another machine?"
   - **Yes** — ask for the repo URL, clone to `~/.claude/imprinted-memories/`, then continue.
   - **No** — create it:
     ```
     gh repo create imprinted-memories --private --description "Cross-machine working memory sync for claude-imprint"
     gh repo clone imprinted-memories ~/.claude/imprinted-memories
     ```
     Scaffold the directory structure:
     ```
     machines/
     universal/
     .gitignore  (containing .DS_Store)
     ```
     Initial commit and push, then continue.

   If scaffolding a new repo and `~/.claude/working-memory/team/` exists, also create
   a `team/` directory in the repo structure.

## Process

1. **Snapshot** — Get the machine hostname via `hostname -s`. Copy all files from
   `~/.claude/working-memory/` to `~/.claude/imprinted-memories/machines/{hostname}/`.

1b. **Team snapshot** — If `~/.claude/working-memory/team/` exists with `.md` files,
    copy its contents to `~/.claude/imprinted-memories/team/`. This is a shared directory
    (not per-machine), so merge rather than overwrite: only add entries that don't already
    exist in the repo's team files. Include team changes in the snapshot commit.
   Write a `.snapshot-date` file containing today's date (YYYY-MM-DD).
   Stage, commit with message "snapshot {hostname} YYYY-MM-DD", and push.

2. **Pull** — Run `git pull --ff-only` in the imprinted-memories repo. If it fails due
   to diverged history, tell the user how to resolve manually and stop.

3. **Inventory** — List all directories under `machines/`. If only the current machine
   exists, report "Nothing to compare yet — run /distill from another machine first" and stop.

4. **Analyze** — Read all machine snapshots. Classify each entry:
   - **Universal**: present on 2+ machines with the same intent (core patterns)
   - **Machine-specific**: unique to one machine
   - **Contradictions**: same topic, different conclusions across machines
   - **Stale**: snapshot `.snapshot-date` older than 14 days

5. **Check existing universals and team patterns** — Read `universal/` and `team/` files.

   For team patterns, compare the repo's `team/` files against local `working-memory/team/`:
   - New entries from other team members to pull into local
   - Local entries not yet pushed to the repo
   - Conflicts where the same topic differs

   For universals, compare against the analysis:
   - New promotions: universal patterns not yet in `universal/`
   - Updates needed: universal entries that have evolved on machines
   - Regressions: universals that have been removed or contradicted on machines

6. **Present findings** in this format:

   ### Machines
   (Table: hostname, snapshot date, entry count, staleness)

   ### Universal Patterns
   (Patterns found on 2+ machines — candidates for promotion)

   ### Machine-Specific
   (Patterns unique to one machine — informational only)

   ### Contradictions
   (Same topic, different conclusions — needs human decision)

   ### Staleness Warnings
   (Machines with snapshots older than 14 days)

   ### Proposed Actions
   (Numbered list of concrete changes to `universal/` files)

7. **Wait for approval** — the user may approve all, select by number, or decline.

8. **Apply** — Write approved changes to `universal/` files. Use the same file and section
   structure as the working memory templates (profile.md, collaboration-patterns.md,
   boundaries.md, rules.md, tools.md, anti-patterns.md). Rules promote like any other
   pattern: a rule present in `rules.md` on 2+ machines becomes a universal rule in
   `universal/rules.md`. Stage, commit with message "distill: promote universals YYYY-MM-DD",
   and push.

9. **Offer to seed local** — Ask if the user wants to merge any universal patterns that
   aren't yet in their local `~/.claude/working-memory/` files. On approval, add the
   missing entries using the Edit tool. Do not overwrite existing entries — append to the
   appropriate sections.
