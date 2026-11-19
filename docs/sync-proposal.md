# Miro and repo sync: manifest and GitHub Action (proposal, nothing installed)

## miro-sync.json (lives in the repo root)

{
  "board": "https://miro.com/app/board/[BOARD_ID]/",
  "repo": "DaveRossBuildProjects/[REPO]",
  "branch": "main",
  "lastSyncedCommit": "[SHORT_SHA]",
  "lastSyncedAt": "[ISO_TIMESTAMP]",
  "artifacts": [
    { "id": "architecture", "owner": "repo", "source": "docs/diagrams/architecture.mmd", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "userflow", "owner": "repo", "source": "docs/diagrams/userflow.mmd", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "sequence", "owner": "repo", "source": "docs/diagrams/sequence.mmd", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "endpoints", "owner": "repo", "source": "docs/endpoints.json", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "prototype", "owner": "repo", "source": "docs/prototype/", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "intent", "owner": "repo-pr", "source": "docs/intent.md", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "spec", "owner": "repo-pr", "source": "docs/spec.md", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "plan", "owner": "repo-pr", "source": "docs/plan.md", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "decisions", "owner": "miro", "source": "docs/decisions.json", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "risks", "owner": "miro", "source": "docs/risks.json", "widget": "[WIDGET_ID]", "hash": "[SHA256]" },
    { "id": "roadmap", "owner": "miro", "source": "docs/roadmap.json", "widget": "[WIDGET_ID]", "hash": "[SHA256]" }
  ]
}

Owner values: repo = regenerated from code, repo to Miro only. repo-pr = repo is canonical, but a board edit opens a pull request. miro = board is canonical, exported to the repo by pull request.

## .github/workflows/miro-sync.yml (optional, runs on merges to main)

name: Miro sync
on:
  push:
    branches: [main]
    paths-ignore: ['miro-sync.json']
concurrency:
  group: miro-sync
  cancel-in-progress: true
jobs:
  sync:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v4
        with: { fetch-depth: 0 }
      - name: Anything that affects the board changed?
        id: changed
        run: |
          if git diff --name-only ${{ github.event.before }} ${{ github.sha }} | grep -qE '^(app|src|server|docs|README|FEATURES)'; then echo "run=true" >> $GITHUB_OUTPUT; fi
      - name: Run the sync skill
        if: steps.changed.outputs.run == 'true'
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: "Run the repo-miro-sync skill in repo-to-Miro mode for commit ${{ github.sha }}. Open a pull request for any file changes. Do not delete anything."
        # Miro access: the runner needs a Miro token stored as a repository secret (MIRO_ACCESS_TOKEN).

Notes: the paths filter and the early exit stop the job from spending anything when the change cannot affect the board. The concurrency block prevents overlapping runs. The sync commit touches only miro-sync.json, which is excluded, so it cannot loop. Use of the action's exact inputs and Miro access from a GitHub runner must be verified before adopting.
