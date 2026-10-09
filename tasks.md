# Agent Desk: Tasks

Legend: [x] done, [ ] to do

## Phase 1: Working agent (single index.html)
- [x] Goal input, example goals, Run, Stop, Clear board
- [x] Five lanes: Think, Plan, Use tools, Take actions, Remember
- [x] Sequence numbers so order across lanes is visible
- [x] Tool registry with names, descriptions, input schemas
- [x] Read tools: calculator, convert_units, get_datetime, list_tasks, search_notes
- [x] Action tools: add_task, complete_task, set_timer, save_note, write_output
- [x] Safe calculator (input validated before evaluation)
- [x] Offline engine: goal splitting and rule-based tool matching
- [x] Claude engine: tool-calling loop with submit_plan and an 8-turn limit
- [x] Fallback from Claude to offline when the first call fails
- [x] Approval checkbox before actions
- [x] Memory: recall before a run, save notes and run summaries, persisted in localStorage
- [x] Panels: tasks, timers (live countdown and banner), memory, written output with copy
- [x] Responsive layout, focus styles, reduced-motion support
- [x] requirements.md, readme.md, tasks.md

## Phase 2: Test and polish
- [ ] Open in Chrome, Firefox and Safari
- [ ] Check at 360, 768 and 1280 px widths
- [ ] Run each example three times; confirm lanes, panels and answer agree
- [ ] Test Claude engine with a valid key, an invalid key, and no network
- [ ] Test the approval prompt (approve and cancel)
- [ ] Reload the page and confirm tasks and notes persist
- [ ] Keyboard-only walkthrough of a full run
- [ ] Add rules for more phrasings in the offline engine (dates like "tomorrow", "every day")
- [ ] Export and import tasks and memory as JSON

## Phase 3: Smarter agent
- [ ] Stream the Claude response so thoughts appear as they are written
- [ ] Let the agent revise its plan when a tool fails
- [ ] Add a "reflect" step where the agent checks its own answer
- [ ] Better memory recall using embeddings instead of keyword overlap
- [ ] Let the user edit the plan before the agent runs it

## Phase 4: More tools (behind a backend)
- [ ] Web search tool
- [ ] Calendar tool (read and create events)
- [ ] Email draft and send, always with approval
- [ ] File reader for uploaded documents
- [ ] Backend proxy that holds the API key and logs tool calls

## Phase 5: Multi-agent and autonomy
- [ ] Specialist agents (planner, researcher, writer, reviewer) that hand work to each other
- [ ] Recurring runs on a schedule
- [ ] Spending, step and time limits per run
- [ ] Run history page

## Phase 6: Submission and demo
- [ ] Deploy index.html to GitHub Pages
- [ ] Record a 2-minute walkthrough: run Example 1, show the lanes and panels, then recall a saved note
- [ ] Write a short project blurb from readme.md
- [ ] Final check that all four files are in the repository root
