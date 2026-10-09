# Agent Desk

An AI agent is an LLM that can **think, plan, use tools, take actions and remember information**. Agent Desk lets you watch all five happen. Give it a goal such as:

> Add a task to call the bank by Friday, set a timer for 10 minutes called tea, and calculate 15% of 2400.

and five lanes fill in as it works: what it thinks, the plan it writes, the tools it calls, the actions that change your tasks and timers, and what it remembers.

## Try it
1. Open `index.html` in a browser. No install, no build.
2. Click **Example 1** and press **Run agent**.
3. Watch the lanes. The numbers (#1, #2, ...) show the order of events across lanes.
4. Check the panels below: the task, the running timer, and the saved run in memory.
5. Click **Example 2**, run it, then run **Example 4**. The agent finds the note it saved earlier.

## The five lanes

| Lane | What it shows |
|---|---|
| Think | The agent's reasoning for the goal and for each step |
| Plan | The ordered steps it will carry out |
| Use tools | Each tool call and its result (calculator, converter, clock, list tasks, search notes) |
| Take actions | Changes it made: task added, timer started, note saved, document written |
| Remember | Notes recalled before it starts, and what it saved afterward |

Tools that only read or calculate appear in "Use tools". Tools that change something also appear in "Take actions".

## What it can do
Calculate, convert units, tell the date and time, add and complete tasks, list tasks, start timers, save and search notes, and write a document to the output panel.

Things to try:
- `Convert 100 f to c; compute 2 plus 3 times 4`
- `Remind me in 30 minutes to stretch`
- `Add task: finish report by Monday, add task buy milk`
- `Mark task 1 as done, then list my tasks`
- `Write a short email asking for a deadline extension` (needs the Claude engine for real text)

## Two engines
**Offline agent (default).** Rule-based. It splits your goal into parts and matches each part to a tool using patterns. It is fast and needs nothing, but it only understands phrasing it has rules for. Parts it cannot match are reported as skipped.

**Claude agent (optional).** Paste a Claude API key into the field at the top right. The model then does its own thinking and planning and uses the same tools through real tool calling, so it can handle open-ended goals like "Plan my week from these tasks and write it up." If the call fails before any tool has run, the app falls back to the offline agent and tells you.

- The key stays in the page and is not saved. Only the goal, recalled notes, and tool calls and results are sent to the API.
- A key in a browser is fine for local testing. For anything shared, put the API call behind a backend.

## Memory
- Tasks, notes and run summaries are stored in your browser (localStorage) and survive a reload.
- Timers live only while the page is open.
- Before each run the agent recalls notes and earlier runs that share keywords with your goal. "Forget everything" clears memory.

## Safety
Everything happens inside the page. Nothing is emailed, posted or purchased. Tick **Ask me before it takes an action** to approve each action. The calculator only accepts numbers and arithmetic symbols. Claude runs are capped at 8 turns, and Stop ends a run.

## Files
| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `requirements.md` | Requirements, architecture, acceptance criteria |
| `readme.md` | This file |
| `tasks.md` | Build checklist and roadmap |

## Deploying
Upload `index.html` to any static host (GitHub Pages, Netlify). It is a single file.

## Adding your own tool
Add an entry to the `TOOLS` object in `index.html` with a name, description, input fields, `acts` (true if it changes something) and a `run` function. Both engines can use it: the Claude engine reads the description and schema automatically, and the offline engine needs a matching rule in `mapClause`.

## Next steps
Real integrations (calendar, email, web search) behind a backend with approvals, specialist agents that hand work to each other, and recurring runs. See `tasks.md`.
