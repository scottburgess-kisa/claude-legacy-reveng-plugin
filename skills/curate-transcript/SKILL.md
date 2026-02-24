---
name: curate-transcript
description: Curates a stakeholder interview transcript by removing off-topic content (incumbent team agenda, feature requests, distractions) while preserving application walkthrough and domain knowledge verbatim.
user-invocable: true
allowed-tools: Read, Edit, Bash(mkdir*;cp*)
argument-hint: [transcript-path]
---

You are curating a Defra LAP stakeholder interview transcript. Your job is to remove off-topic passages. All kept text is preserved mechanically — you never write or rewrite any transcript content.

## Input

The transcript file path is: `$ARGUMENTS`

## Steps

1. **Derive the output path** by replacing the `.txt` extension with `_curated.txt` in the same directory. For example:
   - `transcripts/interview-1.txt` → `transcripts/interview-1_curated.txt`
   - `transcripts/deep-dive.txt` → `transcripts/deep-dive_curated.txt`

2. **Copy the original file** to the output path using `cp`. This creates an exact mechanical copy that preserves all text verbatim.

4. **Read the output file** using the Read tool.

5. **Identify passages to remove.** Classify content into two categories:

   **Keep** — do not touch:
   - Application walkthrough content: screen descriptions, user flows, functionality explanations, how the system behaves
   - Domain knowledge: business rules, terminology, processes, regulations, organisational context
   - Any context that helps understand what the application does and why it exists

   **Remove** — these passages must be cut:
   - The incumbent modernisation team's agenda, plans, opinions, or approach discussions
   - Stakeholder feature requests or wishlists for new functionality
   - Tangential conversation not related to the current application or its domain
   - Meta-discussion about the interview process itself (scheduling, logistics, introductions unrelated to the application)

6. **Remove each identified passage** using the Edit tool on the output file. For each passage:
   - Set `old_string` to the **exact text** of the passage to remove (copy it precisely, including whitespace and line breaks)
   - Set `new_string` to an empty string `""`
   - If removing a passage leaves adjacent blank lines, make a follow-up Edit to collapse them to a single blank line

7. **Return a confirmation message** containing:
   - The output file path
   - A brief summary of what categories of content were removed, with approximate counts (e.g. "Removed: 3 passages discussing the incumbent team's migration timeline, 2 feature requests for a new reporting dashboard")
   - If nothing was removed, state that the transcript contained no off-topic material
