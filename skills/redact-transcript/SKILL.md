---
name: redact-transcript
description: Redacts off-topic content (incumbent team agenda, feature requests, distractions) from a stakeholder interview transcript while preserving application walkthrough and domain knowledge verbatim.
user-invocable: true
allowed-tools: Read, Write, Bash(mkdir*)
argument-hint: [transcript-path]
---

You are redacting a Defra LAP stakeholder interview transcript. Your job is to remove off-topic material while preserving application walkthrough content and domain knowledge **word-for-word**.

**CRITICAL: You MUST NOT create summaries, analyses, business documents, or restructured content. Your job is ONLY to remove off-topic sections while preserving everything else exactly as written. Do not paraphrase, reorganise, or rewrite any kept content.**

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

3. **Produce the redacted transcript** following these rules strictly:
   - **NEVER SUMMARISE OR RESTRUCTURE** — the output must remain a transcript, not a business document
   - Retain kept text **word-for-word** — do not paraphrase, summarise, or rewrite any kept content
   - Silently remove off-topic passages — do not insert markers, comments, or placeholders where content was removed
   - Preserve the original text formatting, structure, speaker labels, timestamps, and document layout
   - Keep the conversational flow and original sequence of kept content
   - If removing a passage leaves adjacent blank lines, collapse them to a single blank line

4. **Derive the output path** by inserting `redacted/` after `transcripts/` in the input path. For example:
   - `transcripts/interview-1.txt` → `transcripts/redacted/interview-1.txt`
   - `transcripts/sub/deep-dive.txt` → `transcripts/redacted/sub/deep-dive.txt`

5. **Ensure the output directory exists** by running `mkdir -p` on the parent directory of the output path.

6. **Write the redacted file** to the derived output path using the Write tool.

7. **Return a confirmation message** containing:
   - The output file path
   - A brief summary of what categories of content were removed, with approximate counts (e.g. "Removed: 3 passages discussing the incumbent team's migration timeline, 2 feature requests for a new reporting dashboard")
   - If nothing was removed, state that the transcript contained no off-topic material
