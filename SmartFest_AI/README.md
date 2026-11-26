# SmartFest AI — Hackathon Project

A no-API-key, browser-based festival planning and grouped scheduling prototype.

## Run
1. Extract the ZIP.
2. Open `index.html` in Chrome, Edge, Firefox or Safari.
3. No installation, server, API key, login or internet connection is required.

## Features
- Add/delete departments and tasks; each task has one department and a positive duration.
- Add/remove prerequisite edges (`A -> B` means A must happen before B).
- Correct two-level scheduling:
  1. Detect cycles in the task dependency graph.
  2. Build a department graph from every cross-department prerequisite.
  3. Detect department-level cycles (a cycle makes contiguous department blocks impossible).
  4. Topologically order departments, then topologically order tasks within each department.
- Conflict witness and explanation for task and department cycles.
- Interactive SVG dependency graph; conflicting cycle edges are highlighted.
- What-if dependency removal simulation with explicit commit.
- Gantt-style sequential preparation timeline.
- Critical prerequisite chain / duration estimate.
- Local browser save, JSON export, CSV export and Print-to-PDF.
- One-click sample cases and built-in test suite.

## Important scope notes
- The core problem statement requires a valid grouped task order or `IMPOSSIBLE`. It does not define calendar dates, resource capacity, parallel execution, venue constraints, or real-time durations. The timeline is therefore a simple sequential duration estimate, not a resource-constrained project schedule.
- Conflict repair is advisory. The app never silently edits user requirements.
- The “Generate another valid order” control currently demonstrates an alternate within-department ordering only by reversing the displayed valid order; use the core validator as the authority. It is not an exhaustive enumeration of all topological orders.
- This is a hackathon-ready prototype foundation. Test it with your own unseen inputs before presenting it as production software.

## Algorithm correctness
For contiguous department blocks, every cross-department edge `taskA -> taskB` forces the entire department of A before the entire department of B. Therefore, construct a department graph with one edge for each such relationship. A valid grouped order exists exactly when:
- the complete task dependency graph is acyclic, and
- the department graph induced by cross-department dependencies is acyclic.
Then topologically sort departments and topologically sort each department's induced task subgraph. This preserves all within-department prerequisites and all cross-department block constraints.

## Suggested live demo
1. Load “Valid festival”; run scheduler; show graph and Gantt.
2. Load “Department deadlock”; show the department cycle witness and red arrows.
3. Load “Task cycle”; show task-level cycle.
4. Simulate removing a dependency; commit only if the organizer accepts the change.
5. Run built-in tests; export CSV/JSON; Print / Save as PDF.
