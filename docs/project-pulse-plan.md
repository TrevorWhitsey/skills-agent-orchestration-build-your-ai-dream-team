# Project Pulse Dashboard Plan

## Summary

Build a lightweight static dashboard that helps Mona's contributors scan active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The interface will use responsive project cards, readable status badges, and accessible visual hierarchy. Project information will live in JSON and be rendered by the page.

The required deliverables are `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`. The app will run through VS Code's **Run Project Pulse Dashboard** configuration, serving the `app/` directory and opening `index.html`.

## Agent Responsibilities

- **Orchestrator** coordinates the phases, assigns non-overlapping scopes, tracks dependencies, integrates the results, and reports validation.
- **Planner** defines the file ownership, dependencies, edge cases, and validation criteria in this plan.
- **Designer** provides the visual direction and accessibility recommendations: information hierarchy, card and badge treatment, readable spacing, responsive behavior, and color-independent status/priority cues. Designer returns guidance to the Orchestrator and does not edit implementation files.
- **Coder** implements the four deliverables within the assigned scope, follows the agreed design and data contract, and validates the working app. This follows the Coder agent's runnable-app guidance for `.vscode/launch.json`.

## Implementation Steps

### 1. Agree on the data and design contracts

**Assignments:** Designer provides design guidance; Orchestrator records the shared contracts for Coder. No implementation files are edited in this step.

Designer should specify a compact dashboard hierarchy, responsive project-card layout, visible status and priority treatment, and accessible text/contrast expectations. The implementation should include a clear Project Pulse title, semantic page structure, and understandable empty/error states.

Use a top-level `projects` array in `app/project-data.json`. Every project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a concise `summary` field as well to meet the brief's contributor-friendly summary requirement. Agree on the class hooks `.dashboard` and `.project-card`, plus any additional hooks needed for the cards and badges, before implementation.

### 2. Prepare independent data and launch files in parallel with design

**Assignments:**
- Designer: design guidance only; no files.
- Coder: `app/project-data.json` and `.vscode/launch.json`.

These scopes do not overlap, so the Orchestrator can request Designer's recommendations while Coder prepares the data and launch configuration.

Populate several clearly illustrative sample projects; the brief does not provide real project records. Keep each record's required fields and summary concise. Configure `.vscode/launch.json` as strict JSON with no comments, named **Run Project Pulse Dashboard**, using `python3 -m http.server 5500` with `cwd` set to `${workspaceFolder}/app`. Set `serverReadyAction` to open `http://localhost:%s/index.html`, not the app directory root.

### 3. Implement and integrate the dashboard

**Assignments:** Coder: `app/index.html` and `app/styles.css`. Designer's guidance and the data contract from Step 1 are dependencies.

In `app/index.html`, use the exact title **Project Pulse**, reference `styles.css` and `project-data.json`, and render visible `.project-card` elements from the JSON `projects` array. Show each project's name, owner, status, `recentActivity`, priority, and summary. Load JSON over HTTP so the app works when served by the launch configuration. Use semantic markup and text-safe DOM rendering for data values.

In `app/styles.css`, provide a polished, responsive dashboard with `.dashboard` and `.project-card` selectors, clear visual hierarchy, readable spacing, status badges, and differentiated priority treatment. Include `border-radius` and `box-shadow` for project cards. Status and priority must remain understandable without relying on color alone.

### 4. Integrate, validate, and hand off

**Assignments:** Coder runs implementation checks; Orchestrator reviews the integrated result and reports outcomes. No additional implementation files are expected.

Run the JSON and repository checks, then launch **Run Project Pulse Dashboard** from VS Code's Run and Debug view. Confirm that the browser opens `index.html` and displays the populated dashboard rather than a directory listing. Stop the preview server after the smoke test. Report any remaining limitations, including the illustrative nature of sample data.

## File Assignments and Dependencies

| File or output | Owner | Depends on |
| --- | --- | --- |
| `docs/project-pulse-plan.md` | Planner / Orchestrator | Project brief, exercise steps, and agent definitions |
| `app/project-data.json` | Coder | Agreed data contract; can be prepared in parallel with Designer guidance |
| `.vscode/launch.json` | Coder | Required command, working directory, launch name, and target URL; can be prepared in parallel with Designer guidance |
| `app/index.html` | Coder | Data contract, Designer guidance, and `app/project-data.json` field names |
| `app/styles.css` | Coder | Designer guidance and the agreed HTML/CSS class hooks |
| Integrated preview and report | Orchestrator, with Coder validation | All four implementation files |

## Parallel and Sequential Work

**Can run in parallel:** After the data and selector contracts are stated, Designer can provide visual/accessibility guidance while Coder creates only `app/project-data.json` and `.vscode/launch.json`. These tasks have disjoint file scopes and no data dependency on the design recommendations.

**Must run sequentially:** Coder should implement `app/index.html` and `app/styles.css` after Designer's guidance and the data/class contracts are available. This keeps markup and styling aligned without concurrent edits to either file. Integration and browser validation must wait until all four implementation files are complete.

Do not assign Designer and Coder overlapping edits to the same files. If the Orchestrator changes file ownership, it should do so between phases and communicate the handoff explicitly.

## Edge Cases and Risks

- `fetch()` of `project-data.json` requires an HTTP server; opening `index.html` directly with `file://` is not a supported preview path.
- Handle an empty `projects` array with a useful empty state, and handle a failed JSON request with a visible, understandable error instead of a blank dashboard.
- Render data as text rather than injecting JSON strings as HTML.
- Ensure status and priority labels remain legible at narrow widths and are not conveyed by color alone.
- Port `5500` may already be occupied. Stop the conflicting server before launching; keep the required command and port for the exercise.
- Sample project data is illustrative because the repository brief does not specify Mona's actual project records.

## Validation Expectations

1. Confirm all four required files exist.
2. Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json` to confirm both files parse.
3. Confirm the implementation satisfies the Step 3 checks: exact dashboard title, stylesheet and data references, visible `.project-card` markup with status, `recentActivity`, and priority, `.dashboard` and `.project-card` CSS selectors, `border-radius`, `box-shadow`, and the required project fields under the top-level `projects` key.
4. Confirm the launch configuration is named **Run Project Pulse Dashboard**, serves from `${workspaceFolder}/app` with `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html`.
5. Launch it from VS Code and verify the browser shows rendered project cards and all expected fields. Check both a normal desktop width and a narrow viewport, and verify empty/error states if practical.
6. The exercise's Step 2 workflow checks the plan file and its required planning terms when the plan is pushed. Step 3 checks file presence, expected content, JSON syntax, and launch name/target; it does not run the app. The manual preview is therefore required to verify actual browser behavior.

## Assumptions and Open Questions

- The app is a small static HTML/CSS/JSON experience with no framework, backend, or external data service.
- The brief does not supply real project records or a fixed visual brand. Use clearly illustrative examples and a restrained, accessible visual direction.
- `summary` is an additional project field to satisfy the brief's summary requirement; the five named fields remain required.
- The required launch command, port, working directory, launch name, and URL are fixed by the exercise. If port `5500` is unavailable, the expected first remedy is to stop the conflicting process.
- No further product decisions are needed before implementation; the Orchestrator should ask Mona only if real project data or established brand styling becomes available and must replace the illustrative defaults.