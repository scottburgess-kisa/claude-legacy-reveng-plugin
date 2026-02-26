---
name: validate-mermaid
description: Validates all Mermaid diagram blocks in a markdown file using the Mermaid Chart MCP tool and fixes broken diagrams in place.
user-invocable: true
allowed-tools: Read, Edit, mcp__claude_ai_Mermaid_Chart__validate_and_render_mermaid_diagram
argument-hint: [markdown-file-path]
---

You validate every Mermaid diagram block in a markdown file. For each broken diagram you attempt to fix it in place, retrying up to 2 times.

## Input

The markdown file path is: `$ARGUMENTS`

## Steps

1. **Read the file** using the Read tool.

2. **Extract all fenced Mermaid blocks.** Identify every occurrence of a ` ```mermaid ` code fence and its closing ` ``` `. Note the line numbers and content of each block. If no Mermaid blocks are found, report "No Mermaid diagrams found in [file path]" and stop.

3. **Validate each block** by calling `mcp__claude_ai_Mermaid_Chart__validate_and_render_mermaid_diagram` with:
   - `mermaidCode`: the content between the fences (excluding the fence lines themselves)
   - `prompt`: "Validate this Mermaid diagram"
   - `diagramType`: inferred from the first line of the block (e.g. `flowchart`, `sequenceDiagram`, `gantt`, `classDiagram`)
   - `clientName`: "claude"

   If the MCP tool is unavailable (tool call errors or is not found), report "Mermaid validation skipped — MCP tool unavailable" and stop. Do not fail the caller's workflow.

4. **Fix broken diagrams.** For each block that fails validation:
   - Read the error details from the tool response
   - Determine the fix based on the error message
   - Use the Edit tool to replace the broken mermaid content with the corrected version (use the full block content as `old_string` and the fixed content as `new_string`)
   - Re-validate by calling the MCP tool again
   - If it still fails, attempt one more fix (maximum 2 retries per block)
   - If a block remains broken after 2 retries, mark it as unfixable and continue to the next block

5. **Report results.** Return a summary containing:
   - Total number of Mermaid blocks found
   - Number that passed validation on first attempt
   - Number that were fixed (with a brief note of what was wrong)
   - Number that remain broken after retries (with the error details)
