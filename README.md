# repo-miro-sync

A Claude plugin that documents a GitHub repository on a Miro board and keeps the two in sync. It is written for non-technical owners of a codebase, such as product managers, evangelists, analysts and founders.

## What it does

The plugin has two skills.

**document-repo-on-miro** takes a repo address and a Miro board address and builds eleven artifacts on the board: an architecture diagram, a user flow diagram, a sequence diagram, intent.md, spec.md, plan.md, a clickable prototype of the screens, a decision log, a roadmap kanban, an endpoint matrix, and a risk and security register. It labels what it confirmed in the code and what it inferred, and it uses sample data only.

**repo-miro-sync** keeps the board and the repo current using a manifest file, miro-sync.json, stored in the repo. Artifacts that come from code are regenerated when the code changes. Edits to documents, decisions, risks and the roadmap on the board come back to the repo as a pull request that a person approves. If both sides changed the same item, it reports a conflict and changes nothing.

A sample of the output, built from a fictional app, is in the Miroverse template for this plugin.

## Requirements

- Claude with the Miro connector turned on and permission to edit the target board.
- Claude access to GitHub: the GitHub connector for private repos, or a public repo.
- A Miro board you can edit.

## How to use

1. Add the plugin from the Claude directory once it is published.
2. Connect Miro and GitHub in Claude settings.
3. Say: document this repo on this board, and give the two addresses.
4. Later, say: create the sync manifest, then merge the pull request it opens.
5. To sync, say: run the repo sync. Say dry run first to preview. Ask Claude to schedule it weekly if you want it automatic.

## Data handling

This section answers the directory's data handling questions.

- **What the plugin reads.** Through the user's own connectors, it reads the contents of the repo the user names (source files, configuration, docs, history metadata) and the contents of the Miro board the user names. It may therefore process whatever those contain.
- **What the plugin writes.** Miro board items on the named board, and, for sync, a branch and pull request on the named repo. It never pushes to the default branch.
- **Personal data.** The plugin does not collect personal data about its users. Its instructions tell Claude never to copy secrets or real personal data found in a repo into the board, documents or reports, to use sample data in prototypes and examples, and to report where such data was found without repeating it.
- **Where data goes.** Nowhere beyond the user's Claude session and the user's own Miro and GitHub accounts. The plugin has no server, no analytics, no telemetry and no third-party service. It does not bundle an MCP server and does not send data to any destination other than those connectors.
- **Storage and retention.** The plugin stores nothing itself. The only persisted artifacts are the board items and repo files the user asks for, which live in the user's own Miro and GitHub accounts and follow their retention settings. The plugin has no retention period of its own.
- **Deletion.** The skills never delete files, branches or board items without asking the user each time.
- **Age.** A general-purpose business tool. It is not directed at people under 18.
- **Scheduled and automated runs.** A scheduled run uses the user's own Claude session and connectors and the same rules. An optional GitHub Action (see docs) needs the user's own API key and Miro token stored as repository secrets, which the plugin never reads or prints.

## Cost

The plugin is free. Using it consumes your own Claude plan usage. A sync run with no changes stops early. The optional GitHub Action also bills GitHub runner minutes on private repos beyond the included allowance, plus API usage per token.

## Safety notes

Documentation of a real repo can describe security weaknesses. Review the board before sharing it widely.

## Docs

docs/sync-proposal.md describes the manifest and an optional GitHub Action.

## License

MIT. See LICENSE.
