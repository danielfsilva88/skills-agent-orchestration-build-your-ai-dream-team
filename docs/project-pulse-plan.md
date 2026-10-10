# Project Pulse implementation plan

## Summary and goal

Build a lightweight, static Project Pulse dashboard that lets Mona's contributors scan active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The first view should read as a polished, responsive dashboard rather than a plain page or a server directory listing. Keep the implementation dependency-free and use the repository's requested Copilot CLI orchestration flow: the Orchestrator delegates visual/accessibility decisions to Designer and implementation to Coder.

The brief specifies three app files and a VS Code launch configuration. The Step 3 exercise further requires the exact page title `Project Pulse`, visible cards using `project-card`, the `.dashboard` and `.project-card` CSS selectors, a top-level `projects` JSON array with `name`, `owner`, `status`, `recentActivity`, and `priority`, and a launch configuration named `Run Project Pulse Dashboard`.

## Ordered phases and file assignments

1. **Confirm contracts and ownership — Orchestrator**
   - Use this plan to assign the work and keep file ownership explicit.
   - **Files:** no implementation files.
   - Confirm the data shape, required UI hooks, launch behavior, and validation criteria below before delegating.

2. **Define layout, information hierarchy, and accessibility — Designer**
   - Decide a clear dashboard hierarchy: page title and concise introduction, then a responsive collection of project cards.
   - Specify how each card presents the project name, owner, status, recent activity, priority/risk, and a short summary. The brief calls for the summary; the Step 3 required data schema does not define a summary field, so use an optional `summary` field or concise static description without making it a required schema field.
   - Recommend accessible semantic structure, readable contrast and spacing, visible status/priority distinctions that do not rely on color alone, and narrow-screen behavior.
   - **File ownership:** design guidance only; do not edit implementation files unless the Orchestrator explicitly assigns them.

3. **Implement dashboard markup and styling — Coder, informed by Designer**
   - Build semantic page markup and render project cards from `project-data.json`; display every required field and a useful summary.
   - Implement the polished visual direction, with responsive layout and the required deterministic `.dashboard` and `.project-card` hooks.
   - **Files:** `app/index.html` (Coder); `app/styles.css` (Coder, incorporating Designer's decisions).
   - The page title must be exactly `Project Pulse`, the stylesheet must be linked, and the page must load the JSON data rather than hard-code project cards. Handle loading and failure states accessibly.

4. **Provide representative project data — Coder**
   - Create a JSON document with a top-level `projects` array and several plausible, contributor-friendly sample projects. Each project must have `name`, `owner`, `status`, `recentActivity`, and `priority`; include a brief `summary` if chosen in Phase 2.
   - **File:** `app/project-data.json`.
   - Keep values concise and consistent with the UI's supported statuses and priorities; the JSON must be valid and must not require a framework or build step.

5. **Configure the runnable preview — Coder**
   - Add a strict JSON VS Code launch configuration named `Run Project Pulse Dashboard`.
   - Serve the `app/` directory with `python3 -m http.server 5500`, set the working directory to `${workspaceFolder}/app`, and use `serverReadyAction` to open `http://localhost:%s/index.html`. Opening `index.html` is essential: opening only the directory root risks showing a directory listing instead of the dashboard.
   - **File:** `.vscode/launch.json`.

6. **Integrate and validate — Orchestrator and Coder**
   - Confirm all assigned files work together, run the repository validator, inspect the rendered dashboard, and report changed files, validation, and any unresolved risk. Do not stage, commit, or push as part of implementation; the learner controls Git operations.
   - **Files:** validate the four deliverables; make follow-up changes only within their assigned file scopes.

## Dependencies and work ordering

- Phase 1 precedes delegation so Designer and Coder receive a stable contract.
- Designer's design decisions and Coder's initial markup/data work can proceed in parallel after the contract is assigned. Coder should use the required fields and hooks immediately; coordinate any optional summary-field decision before finalizing the rendering logic.
- CSS implementation depends on Designer's visual/accessibility guidance, but can start against the agreed card structure and `.dashboard` / `.project-card` hooks.
- The launch configuration is independent of visual design and can be implemented alongside the app, but end-to-end preview validation must wait until `app/index.html`, `app/styles.css`, and `app/project-data.json` exist.
- Integration and validation are sequential and last: validate JSON and launch configuration, then run the preview and confirm the browser shows populated dashboard UI rather than a directory listing.

## Dependencies and repository conventions

- The brief and Step 3 exercise describe a small static app. No application framework, package installation, build command, or external runtime library is required.
- Use the Python 3 standard-library HTTP server specified in Step 3. Fetching a JSON file from a page opened directly with `file://` can fail in browsers; use the HTTP launch configuration for preview and verification.
- `.vscode/tasks.json` currently defines the folder-open task **Open Copilot CLI exercise terminal** (`bash .devcontainer/postStart.sh`). It is not the dashboard preview task and should remain unchanged; use the distinct Run and Debug launch configuration for Project Pulse.
- `scripts/validate-exercise.sh` checks repository/exercise conventions, including JSON parsing for `.vscode/tasks.json`, required exercise guidance, and that learner answer files (including this plan and the app/launch deliverables) are not tracked in the template. It does not replace checking the dashboard's runtime behavior. Do not alter the script or task configuration for this implementation.

## Edge cases and open questions

- **Data load failure or empty list:** show a clear, readable message instead of a blank dashboard; handle an empty `projects` array without rendering broken cards.
- **Missing or malformed fields:** avoid rendering `undefined` or misleading badges. Either validate required values before rendering or provide a safe fallback, while keeping the supplied sample data complete.
- **Status and priority values:** choose a small, consistent set of sample values and ensure every state has readable text and sufficient contrast. No exhaustive enum is specified in the brief.
- **Long names/activity:** allow text to wrap without causing horizontal overflow or hiding important card content.
- **Responsive and assistive use:** ensure cards reflow at narrow widths, semantic headings are ordered, controls (if any) are keyboard accessible, and status/risk is conveyed by text as well as color.
- **Summary field:** the brief requests a short summary but omits it from the required five-field schema. Decide whether to add optional `summary` values to each project or present a concise descriptive line without treating it as mandatory.
- **Browser launch support:** the requested launch configuration must use a VS Code-supported launch type and valid `serverReadyAction` settings for the environment. Confirm the preview launches on port 5500 and opens the explicit `/index.html` URL; account for the port already being occupied if the server cannot start.

## Validation expectations

1. Run `bash scripts/validate-exercise.sh` from the repository root and resolve any failures without changing unrelated exercise files.
2. Parse `app/project-data.json` and `.vscode/launch.json` as JSON; confirm `projects` is an array and every entry has all five required fields.
3. Inspect `app/index.html` for the exact title, stylesheet and JSON references, and visible rendered cards with the `project-card` class showing status, recent activity, and priority. Confirm `app/styles.css` contains `.dashboard` and `.project-card` and provides polished card treatment, readable spacing, and responsive behavior.
4. In VS Code, select **Run Project Pulse Dashboard** and verify the browser opens `http://localhost:5500/index.html` with populated Project Pulse UI. Confirm it is not a directory listing and check the layout at a narrow viewport and the data-load failure/empty state if practical. Stop the preview server when finished.
