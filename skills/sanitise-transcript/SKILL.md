---
name: sanitise-transcript
description: Sanitises a stakeholder interview transcript by removing off-topic content (incumbent team agenda, feature requests, distractions) while preserving application walkthrough and domain knowledge verbatim.
user-invocable: true
allowed-tools: Read, Write, Bash(mkdir*)
argument-hint: [transcript-path]
---

You are sanitising a Defra LAP stakeholder interview transcript. Your job is to remove off-topic material while preserving application walkthrough content and domain knowledge **word-for-word**.

## Input

The transcript file path is: `$ARGUMENTS`

## Steps

1. **Read the transcript** using the Read tool. The path is relative to the project root.

2. **Analyse the content** and classify every passage into one of two categories:

   **Keep** — retain these passages exactly as written:
   - Application walkthrough content: screen descriptions, user flows, functionality explanations, how the system behaves
   - Domain knowledge: business rules, terminology, processes, regulations, organisational context
   - Any context that helps understand what the application does and why it exists

   **Remove** — silently cut these passages:
   - The incumbent modernisation team's agenda, plans, opinions, or approach discussions
   - Stakeholder feature requests or wishlists for new functionality
   - Tangential conversation not related to the current application or its domain
   - Meta-discussion about the interview process itself (scheduling, logistics, introductions unrelated to the application)

3. **Produce the sanitised transcript** following these rules strictly:
   - Retain kept text **word-for-word** — do not paraphrase, summarise, or rewrite any kept content
   - Silently remove off-topic passages — do not insert markers, comments, or placeholders where content was removed
   - Preserve the original markdown formatting, heading structure, speaker labels, and document layout
   - If removing a passage leaves adjacent blank lines, collapse them to a single blank line

4. **Derive the output path** by inserting `sanitised/` after `transcripts/` in the input path. For example:
   - `transcripts/interview-1.md` → `transcripts/sanitised/interview-1.md`
   - `transcripts/sub/deep-dive.md` → `transcripts/sanitised/sub/deep-dive.md`

5. **Ensure the output directory exists** by running `mkdir -p` on the parent directory of the output path.

6. **Write the sanitised file** to the derived output path using the Write tool.

7. **Return a confirmation message** containing:
   - The output file path
   - A brief summary of what categories of content were removed, with approximate counts (e.g. "Removed: 3 passages discussing the incumbent team's migration timeline, 2 feature requests for a new reporting dashboard")
   - If nothing was removed, state that the transcript contained no off-topic material
