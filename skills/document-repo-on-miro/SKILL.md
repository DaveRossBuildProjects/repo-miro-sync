---
name: document-repo-on-miro
description: Document an existing GitHub repo on a Miro board for a non-technical owner. Builds architecture, user flow and sequence diagrams, intent.md, spec.md, plan.md, a clickable prototype, a decision log, roadmap, endpoint matrix and risk register. Use when the user gives a repo and a Miro board and wants it documented.
---

The user gives you a repo address and a Miro board address (ask only if either is missing). Follow the brief below exactly. Treat the user as a non-technical owner.

I am not a developer. I own or have inherited a software project and I need it documented so that I, my colleagues and an AI coding assistant can all understand it, make decisions about it, and rebuild or extend it. Please act as a patient senior engineer and technical writer who explains things in plain language.

REPO: the repo address the user gave you
MIRO BOARD: the board address the user gave you

HOW TO WORK WITH ME
- Use plain language. The first time you use a technical term, explain it in a few words. Never assume I know what a framework, endpoint, API, token or cache is.
- Do not stall with questions. If something is unclear, make a sensible assumption, say what you assumed, and carry on. Ask me only if the answer genuinely changes the result, for example the repo or board is not reachable.
- You have my permission to use any connected tool (GitHub, Miro, files, shell) without asking me each time. The only exception is deleting anything (files, branches, board items, data): ask me first, every time.
- Never change the repo. Read only. Do not push, commit or edit it.
- Keep me informed with a short task list and short progress notes, not a running commentary.

RULES FOR ACCURACY AND SAFETY
- Read the whole repo before writing: every source file, configuration, build file, test, script, doc, changelog, tracker and image folder. Do not summarize from file names. If the repo is too large to read in full, say so and state exactly what you sampled.
- Everything you write must be traceable to the code. Label each important statement as CONFIRMED (seen in this repo), INFERRED (deduced from code that talks to something outside the repo) or UNKNOWN. Never invent endpoints, features, fields or numbers. Count things by actually counting them.
- Before you claim something is unused, dead or legacy, search the code to prove nothing calls it. Before you claim a document is out of date, show the contradiction.
- Where the docs and the code disagree, the code wins. Report the disagreement.
- Never print or copy secrets (tokens, keys, passwords) or real personal data (names, document numbers, faces in screenshots). In prototypes, examples and sample rows use obviously fake data. If you find secrets or personal data committed to the repo, report where, without repeating the values.
- If part of the system is not in the repo (for example the server the app talks to), say so clearly and draw it as "not in this repo", with only what the repo itself proves.
- Use only real names from the code for files, functions, screens and routes.

STEP 1: DISCOVER (do this silently, then give me a short summary)
Work out: what the product is and who uses it; language, framework and build tooling; the folders and modules; entry points; every screen, page, command or API route; data models; how it talks to other systems; where data is stored and how it is protected; configuration and secrets handling; how it is tested; what is planned (trackers, TODOs); what is half-built, unused or contradictory; and the version and latest commit. Record the version and the short commit identifier, because the documentation is a snapshot of that moment.

STEP 2: BUILD THESE ELEVEN ARTIFACTS ON THE MIRO BOARD

1. ARCHITECTURE DIAGRAM (Mermaid, left-to-right flowchart with nested groups)
Show all the code, not a sketch: one group per layer (entry and state, screens or pages, shared components and theme, data layer, legacy or unused code, and the external server if relevant). Each box is a file or class and lists its functions and methods with their parameter names. Include every endpoint, every storage location (databases, files, preferences, secure key stores), every external system, and who calls whom, with short labels on the arrows. Show a "not in this repo" group for outside systems. Show unused code in gray with dotted arrows. Colors: blue for user interface, green for state and routing, yellow for the data layer, orange for storage, purple for outside servers, gray for legacy. Add that color legend as text under the board title.

2. USER FLOW DIAGRAM (Mermaid, top-to-bottom flowchart)
From launch to the most important completed task. Include every decision (as a diamond), every failure and offline or error path, and the exact messages the user sees. Group background work (for example loading data) in its own box. Show how Back or exit behaves. Colors: green for start, end and success, cyan for decisions, yellow for normal steps, orange for warnings and failures.

3. intent.md (plain-language, about one page)
Sections: What it is; The problem; Who it is for; What good looks like; Principles; In scope today; Out of scope for now; Planned next; Risks and open questions.

4. spec.md (detailed requirements an AI coding assistant could rebuild the product from)
Number the sections: platform and build (exact versions, libraries, how secrets are injected); system context; data model (every field and the naming rules); the interface contract (every route or call, request, response, error codes, limits); network and offline behavior; navigation and state; every screen and its exact behavior and wording; every form and its validation rules; theme (exact colors and sizes); security requirements; non-functional requirements; tests (what exists, what it really covers, how to run it); planned requirements; a "Known issues found while reading the code" section; and a final acceptance checklist. Use headings and bullet lists only, no tables and no code fences, so it pastes cleanly into a board document.

5. plan.md (how an AI coding assistant would build it)
Sections: ground rules for every agent; a list of agents, each with what it owns, what it depends on and a "done when" test (include an orchestrator, a scaffold agent, a data and network agent, a design system agent, a screens agent, an app shell agent, an editing agent, a server agent if relevant, a test agent, a review and security agent, and a docs agent, adapted to this repo); build sequence showing what can run in parallel; file ownership so parallel agents never touch the same files; milestones; planned feature work; verification commands copied from the repo; and decisions needed from the owner.

6. CLICKABLE PROTOTYPE of the current screens
One prototype on the board with one screen per distinct screen, including dialogs, forms, empty states and error states. Use the product's real wording, real layout and real colors (read them from the code), the correct device size, and fake sample data only. Every screen must be reachable by links from the first screen and must have a way back. If the product has no screens (for example a command-line tool or API), say so and replace this with a diagram of its commands or routes.

7. DECISION LOG (table)
Columns: ID, Decision, Options considered, Recommendation, Priority (Low, Medium, High), Status (Open, Decided, Deferred), Decision owner. One row for each choice that needs a human, taken from the risks and open questions. Everything starts as Open.

8. ROADMAP (kanban)
Lanes: Backlog, Next, In progress, Done. Columns: Title, Description, Status, Priority, Tags (source of the item: the repo's own tracker, Proposed by you, or Released). Done lane comes from the release history. Mark anything you add yourself as Proposed.

9. ENDPOINT MATRIX (table)
Columns: Path, Method (GET, POST, PATCH, PUT, DELETE), Client function, Triggered from (which screen or action), Server handler, Responses and fallback, Contract status (Confirmed by server code, Inferred from client code, Unconfirmed legacy). One row per route, including routes only used by dead code. If the project has no routes, build the equivalent matrix for its commands, events or public functions.

10. RISK AND SECURITY REGISTER (table)
Columns: ID, Risk, Where in the repo, Impact, Likelihood (Low, Medium, High), Severity (Low, Medium, High), Mitigation, Linked decision, Status (Open, In progress, Mitigated). Look at least for: secrets inside the app or repo; missing lock or timeout; unencrypted traffic; backups; real personal data in the repo; tests that do not test the real code; dead code that still logs or leaks; steps that are not atomic; code outside the repo that the app depends on; and docs that contradict the code. Every risk links to a decision in the log.

11. SEQUENCE DIAGRAM (Mermaid)
The most important write or transaction, from the user's tap to the stored result and the screen refresh. Name the real functions as participants and messages. Show validation, the offline or failure guard, the call to the server, rejection branches, the success branch, and the refresh. Use alt, opt and par blocks, numbered steps, and one note for connection loss.

STEP 3: BOARD LAYOUT AND STYLE
- Put a large title at the top, a subtitle with a one-line description, the version, the short commit and the repo address, then a contents line where every item is a clickable link to its section (use the board's "move to item" links). Number every section heading.
- Order: 1 Architecture, 2 User flow, 3 intent.md, 4 spec.md, 5 plan.md, 6 Prototype, then 7 to 11 to the right of the documents, stacked top to bottom. Documents (intent, spec, plan) sit side by side, about 300 pixels apart.
- Leave generous space between sections. Diagram sizes are not reported by the board, so keep large gaps and never let two items overlap. After every create step, check the reported sizes and move things if needed.
- Use Miro's default "Bright Paper" style: flat, calm, one font, short text, no emojis, no dashes used as punctuation in board text.
- Build the board in stages (title and diagrams, then documents, then prototype, then tables, then sequence), reading back the results each time.

TECHNICAL NOTES FOR MIRO (you handle these, I do not need to)
- Load Miro's canvas composer guidance and format guidance before creating anything.
- Mermaid: give every colored class a fill, stroke and text color; use plain-text arrow labels; keep group ids free of hyphens; escape the greater-than sign inside arrows.
- Documents: Miro documents are about 784 pixels wide and do not render pipe tables, so tables go in native table widgets and code samples stay short.
- Tables and kanban use native Miro widgets with typed columns (text, select with named colors).
- Prototype: one HTML screen per screen, linking by screen name, with the correct device size.
- If the board rejects something as too large, split it into numbered parts rather than shortening it.

STEP 4: FILES AND FINAL REPORT
- Also give me intent.md, spec.md and plan.md as real files I can download, with exactly the same content as the board.
- Before you finish, check your own work: re-search the code for every "unused" or "legacy" claim, confirm counts, and confirm each endpoint and function name exists. Fix anything that fails.
- Finish with a short plain-language report: what you made, the five most important findings, what is confirmed versus inferred, anything you could not do, and the extra artifacts you would propose next. Do not paste the artifacts into chat. Do not repeat the board link more than once.
