# Agent Desk: Requirements

## 1. Purpose
Agent Desk is a small app that shows a working AI agent. An AI agent is an LLM that can **think, plan, use tools, take actions and remember information**. The user gives a goal in plain English, and the app shows each of those five capabilities as it happens, then leaves real results behind (tasks, timers, notes, written output).

## 2. Users
- Students and developers who want to see an agent loop run end to end.
- Anyone who wants a simple personal assistant for tasks, timers, quick maths, conversions and notes.

## 3. The five capabilities
| Capability | What it means here | Where it shows |
|---|---|---|
| Think | Reasons about the goal and each step before acting | Think lane |
| Plan | Turns the goal into an ordered list of steps | Plan lane |
| Use tools | Calls functions that calculate, look up or read | Use tools lane |
| Take actions | Calls functions that change something (add a task, start a timer, save a note, write a document) | Take actions lane and the panels below the board |
| Remember | Recalls relevant notes and past runs before starting, saves new facts and a run summary afterward | Remember lane and the Long-term memory panel |

## 4. Functional requirements
### Goal input
- FR-1 The user can type a goal and run the agent; example goals are provided.
- FR-2 The user can stop a run in progress.
- FR-3 The user can optionally require approval before each action runs.

### Agent loop
- FR-4 The agent writes a thought, then a plan, then repeats "call a tool, read the result, think" until all steps are done.
- FR-5 Every tool call and its result is shown with a sequence number so the order across lanes is clear.
- FR-6 The agent ends with a final answer summarizing what it did.
- FR-7 A tool error is shown and passed back to the agent instead of crashing the run.

### Two engines
- FR-8 Offline engine (no key): rule-based. It splits the goal into parts, matches each part to a tool, and runs them in order. Parts it cannot match are reported as skipped.
- FR-9 Claude engine (API key entered): uses the Claude Messages API with real tool calling. The model writes its reasoning, calls a `submit_plan` tool, then calls the app's tools until done (limit 8 turns).
- FR-10 If the Claude call fails before any tool has run, the app switches to the offline engine and says so. If it fails after tools have run, the run stops and says so.

### Tools
| Tool | Type | Behavior |
|---|---|---|
| calculator | read | Evaluates arithmetic; input restricted to digits and `+ - * / ( ) % ^` |
| convert_units | read | Length, mass, volume and temperature |
| get_datetime | read | Current local date and time |
| list_tasks | read | Lists tasks with ids and status |
| search_notes | read | Keyword search over saved notes |
| add_task | action | Adds a task with optional due text |
| complete_task | action | Marks a task done by id or title |
| set_timer | action | Starts a countdown (0 to 24 hours) with an in-page alert |
| save_note | action | Saves a fact to long-term memory |
| write_output | action | Puts a written document in the output panel with a copy button |

### Memory
- FR-11 Before each run the agent recalls up to 3 notes and 1 earlier run that share keywords with the goal, and shows them.
- FR-12 After each run a summary is saved. Notes and runs persist in browser storage.
- FR-13 The user can delete single notes or clear all memory.
- FR-14 Recalled notes are given to the Claude engine as context.

### World panels
- FR-15 Tasks: view, mark done or reopen, delete. Persisted.
- FR-16 Timers: live countdown, banner when finished, remove. Not persisted.
- FR-17 Written output: view and copy.

## 5. Non-functional requirements
- NFR-1 Single index.html, no build step. Works offline except web fonts and the optional API call.
- NFR-2 All data stays in the browser. With an API key, only the goal, recalled notes, tool calls and tool results are sent to the API.
- NFR-3 The API key is read from the input field, never stored.
- NFR-4 The calculator must not execute arbitrary code (input is validated before evaluation).
- NFR-5 Responsive from 360 px wide; keyboard accessible with visible focus; respects reduced motion.
- NFR-6 A typical offline run of three steps completes in under 5 seconds.

## 6. Architecture
```
index.html
 ├─ UI: goal bar, five lanes, answer box, world panels
 ├─ Tool registry: name, description, JSON schema, run() (shared by both engines)
 ├─ execTool(): logs the call, honors approval, runs the tool, logs the result
 ├─ Offline brain: splitGoal() + mapClause() rules
 ├─ Claude brain: tool-use loop against the Messages API
 └─ Memory: localStorage (notes, runs, tasks)
```

## 7. Agent loop (Claude engine)
1. Recall memory, show it, add it to the system prompt.
2. Send goal + tool definitions.
3. For each reply: show text as Think; run `submit_plan` into Plan; run other tools through execTool.
4. Send tool results back; repeat until the model replies with no tool calls or 8 turns pass.
5. Show the final answer, save a run summary.

## 8. Safety
- Tools only change data inside the page. No email, messages or purchases.
- The approval checkbox lets the user veto each action.
- A browser-held API key is for local testing only. Real deployments need a backend proxy.
- Turn limit (8) and the stop button bound a run.

## 9. Acceptance criteria
- Example 1 produces a task (due Friday), a 10-minute timer called tea, and the answer 360, with each step visible in the lanes.
- Running "What do you remember about marathon?" after Example 2 finds the saved note.
- Ticking the approval box and clicking Cancel on the prompt blocks the action and shows "You blocked".
- An unrecognized goal ("Tell me a joke") is reported as skipped, not silently ignored.
- Entering an invalid API key shows the failure and falls back to the offline engine.
- Tasks and notes are still there after reloading the page; timers are not.
- No horizontal page scroll at 360 px width.

## 10. Out of scope
Real integrations (email, calendar, web search), multi-agent collaboration, background or scheduled runs when the page is closed, user accounts, voice input.
