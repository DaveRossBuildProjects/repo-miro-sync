---
name: repo-miro-sync
description: Keep a Miro documentation board and its source repo in sync using a miro-sync.json manifest. Repo-owned artifacts are regenerated on the board; Miro-owned artifacts and board edits to documents come back to the repo as pull requests. Use for scheduled or on-demand sync of a repo documentation board, or to create the manifest.
---

# Repo and Miro sync

Run in one of three modes: repo-to-miro, miro-to-repo, or both (default). If the user says dry run, report what would change and change nothing.

## Rules
- Read the repo and the board only until a change is approved. Never push to the default branch. All repo changes go on a branch named miro-sync/YYYY-MM-DD with a pull request.
- Never delete files, branches, board items or data without asking the owner each time. Mark removed items as Removed or flag them in the report instead.
- Never print or copy secrets or real personal data. Use sample data only.
- If both sides changed an artifact since the last sync, do not overwrite either side. Flag a conflict in the report.
- State assumptions and proceed rather than asking for ordinary tool permission.
- Match the board's existing style. No emojis, no em dashes.

## Steps
1. Load miro-sync.json from the repo root (template in references/miro-sync.template.json). If it is missing, offer to create it from the board by matching artifact titles to widget IDs, and open a pull request with it.
2. Get the current commit. If it equals lastSyncedCommit and no board changes are found, report No changes and stop.
3. Read the board with the Miro tools (read the widget IDs in the manifest). Compute a content hash per artifact and compare with the manifest hash and with the repo source file.
4. Classify each artifact by owner:
   - repo: if source files changed since lastSyncedCommit, regenerate the artifact from the code (Mermaid diagram, endpoint table, prototype screens) and update the widget in place. Re-verify any unused or legacy claim by searching the code before writing it. If the board copy was edited by hand since the last sync, record that in the report before overwriting.
   - repo-pr: the repo file is canonical. If the repo file changed, update the board document. If the board document changed, write the board text to the repo file on the sync branch and open a pull request titled Board edit to [artifact].
   - miro: the board is canonical. Export the table or kanban rows to the JSON source file on the sync branch and open a pull request. Check for conflicts first.
5. Update the board subtitle with the new version, short commit and date. Update the manifest hashes, lastSyncedCommit and lastSyncedAt on the sync branch.
6. Keep the board layout rules: documents 784 wide, generous gaps, check rendered bounds after each update, no overlap.
7. Finish with a short plain-language report: what changed in each direction, pull request links, conflicts that need a human, claims re-verified, anything that could not be done.

## Failure handling
If a Miro call fails, retry once, then record the failure and continue with other artifacts. A partial run is acceptable; the manifest records only artifacts that were actually synced.
