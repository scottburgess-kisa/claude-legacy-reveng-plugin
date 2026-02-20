---
name: extract-workflows
description: Extracts user workflows from a redacted interview transcript, cross-referencing HTML screen representations to produce documented workflows with mermaid diagrams.
user-invocable: true
allowed-tools: Read, Write, Bash(mkdir*), Glob
argument-hint: [redacted-transcript-path]
---

You are extracting user workflows from a redacted Defra LAP stakeholder interview transcript. You will cross-reference the transcript with HTML screen representations to produce one documented workflow file per distinct workflow.

## Input

The redacted transcript file path is: `$ARGUMENTS`

## Steps

1. **Read the transcript** using the Read tool. The path is relative to the project root.

2. **Discover all HTML screen files** using the Glob tool with the pattern `html/**/*.html`. These are HTML representations of legacy application screens.

3. **Read each discovered HTML file** using the Read tool to understand the available screens — their names, structure, key fields, and purpose.

4. **Analyse the transcript** to identify distinct user workflows. A workflow is a sequence of steps a user follows to complete a business task (e.g. "Record a cattle movement", "Run a disease trace-back report"). Look for:
   - Explicit descriptions of tasks or processes the user performs
   - Sequences of screens navigated in order
   - Decision points where the user takes different paths
   - Start and end conditions for each task

5. **Cross-reference each workflow with the HTML screens.** For each step in a workflow, determine whether one of the discovered HTML files corresponds to that screen. Match by screen name, described purpose, or key fields/elements mentioned in the transcript.

6. **For each identified workflow, produce a markdown file** containing:

   - A level-1 heading with the workflow name (e.g. `# Record a Cattle Movement`)

   - A mermaid flowchart (`flowchart TD`) showing the sequence of screens and steps. Use descriptive node labels and reference screen names from the HTML files where matched. Include decision points as rhombus nodes and outcomes/end states as rounded rectangles. Example structure:

     ````markdown
     ```mermaid
     flowchart TD
         A[Dashboard] --> B[Select Record Movement]
         B --> C{Movement Type?}
         C -->|On-farm| D[On-farm Movement Form]
         C -->|Off-farm| E[Off-farm Movement Form]
         D --> F[Confirmation]
         E --> F
     ```
     ````

   - A level-2 heading `## Screen Notes` followed by a subsection for each screen or step in the workflow. Each subsection should include:
     - The screen or step name as a level-3 heading
     - A brief description of what the screen shows or what the user does at this step
     - Key fields, actions, or controls available
     - Any business rules, validation logic, or constraints mentioned in the transcript
     - If an HTML file corresponds to this step, note the path for cross-reference (e.g. `**HTML reference:** html/record-movement.html`)

7. **Derive output filenames** as kebab-cased workflow names under `workflows/`. For example:
   - "Record a Cattle Movement" → `workflows/record-cattle-movement.md`
   - "Disease Trace-back Report" → `workflows/disease-trace-back-report.md`

   One transcript may yield multiple workflow files.

8. **Ensure the output directory exists** by running `mkdir -p workflows`.

9. **Write each workflow file** to its derived path using the Write tool.

10. **Return a confirmation message** listing:
    - Each workflow file written (full path)
    - A one-line summary of what the workflow covers
    - The total number of workflows extracted
