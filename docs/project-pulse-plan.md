# Project Pulse implementation plan

## Goal

Build a lightweight, static Project Pulse dashboard that helps contributors see active projects, their owners, status, recent activity, priority or risk, and a concise summary. The finished dashboard must be polished, accessible, responsive, and runnable from the VS Code **Run Project Pulse Dashboard** configuration.

## Phases and file assignments

1. **Plan the data and experience**
   - **Designer** owns the visual and accessibility direction for `app/styles.css`: content hierarchy, responsive layout, readable typography, status badges, priority treatment, focus states, color contrast, and card affordances.
   - **Coder** owns the data contract for `app/project-data.json`. Create a valid JSON document with a top-level `projects` array. Every project must contain `name`, `owner`, `status`, `recentActivity`, and `priority`; include a concise contributor-friendly summary for display.
   - These assignments can run **in parallel** because the CSS direction does not depend on populated data, and the data schema is defined by the dashboard brief.

2. **Build the dashboard structure**
   - **Coder** owns `app/index.html`. Create semantic, accessible dashboard markup with the exact title **Project Pulse**, a dashboard container, and visible project cards using the `project-card` class.
   - Link `app/styles.css`, load `app/project-data.json`, and render each project’s name, owner, status, recent activity, priority, and summary from the `projects` data.
   - This phase depends on the `app/project-data.json` schema from phase 1. It can start from the agreed Designer hierarchy, but should not modify Designer-owned CSS.

3. **Apply the visual system**
   - **Designer** owns the final `app/styles.css` implementation or review. Include `.dashboard` and `.project-card` selectors; create polished cards with `border-radius`, `box-shadow`, clear status badges, priority emphasis, responsive spacing, and accessible contrast and focus treatment.
   - This phase can begin with the phase 1 direction, but final styling should follow the completed `app/index.html` structure to avoid mismatched selectors.

4. **Create the runnable preview**
   - **Coder** owns `.vscode/launch.json`. Add a strict JSON configuration named **Run Project Pulse Dashboard** that runs `python3 -m http.server 5500` from `${workspaceFolder}/app`.
   - Configure `serverReadyAction` to open `http://localhost:%s/index.html`, ensuring the browser opens the dashboard rather than a directory listing.
   - This work can run **in parallel** with phases 2 and 3 because it has no dependency on their file contents.

5. **Integrate and validate**
   - The **Orchestrator** reviews the combined result for file-scope conflicts, verifies that the data renders into visible cards, confirms the styling matches the Designer direction, and reports blockers or final handoff details.
   - This phase is **sequential** and begins only after the Coder and Designer work is complete.

## Dependencies and ordering

| Work item | Depends on | Reason |
| --- | --- | --- |
| `app/project-data.json` | Project brief | The brief defines the required top-level `projects` array and fields. |
| `app/index.html` | Data schema | Rendering must use the agreed project fields. |
| `app/styles.css` final pass | `app/index.html` structure | Selectors and responsive behavior must match the completed markup. |
| `.vscode/launch.json` | Project brief | It only needs the prescribed app directory, command, port, and target URL. |
| Integration validation | All four assigned files | The dashboard can only be verified after the data, markup, styling, and launch configuration are present. |

## Parallel work decisions

- **Parallel:** Designer’s initial visual/accessibility direction and Coder’s `app/project-data.json`.
- **Parallel:** Coder’s `.vscode/launch.json` and all dashboard implementation work.
- **Sequential:** Coder completes `app/index.html` after the data schema is agreed; Designer’s final CSS pass follows the markup structure; integration validation follows all implementation work.
- **Avoid overlapping ownership:** Designer owns `app/styles.css`; Coder owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.

## Edge cases and risks

- Use a visible empty or error state if `project-data.json` cannot be loaded or has no projects; do not leave the dashboard silently blank.
- Ensure status and priority are not conveyed by color alone, and preserve readable contrast and keyboard focus indicators.
- Escape or render data safely so project values cannot become unintended markup.
- Keep the dashboard useful on narrow screens by allowing cards and metadata to reflow.
- Keep JSON and launch configuration strict: no comments or trailing commas.
- Confirm the launch configuration serves `app/` and opens `index.html`, not the HTTP server’s directory root.

## Validation expectations

1. Confirm all assigned files exist: `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
2. Verify that `app/project-data.json` parses as JSON, has a top-level `projects` array, and every entry has `name`, `owner`, `status`, `recentActivity`, and `priority`.
3. Verify that `app/index.html` includes **Project Pulse**, references `styles.css` and `project-data.json`, and renders visible `.project-card` elements containing status, recent activity, and priority.
4. Verify that `app/styles.css` contains `.dashboard` and `.project-card`, responsive behavior, `border-radius`, `box-shadow`, and accessible focus and contrast treatment.
5. Verify that `.vscode/launch.json` parses as strict JSON, names **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app`, runs `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html`.
6. Run the launch configuration, confirm that it opens the Project Pulse dashboard at `app/index.html`, visually review the desktop and narrow-screen layouts, then stop the preview server.
