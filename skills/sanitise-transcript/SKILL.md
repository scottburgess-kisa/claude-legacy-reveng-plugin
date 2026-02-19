---
name: sanitise-transcript
description: Sanitises a stakeholder interview transcript by removing off-topic content (incumbent team agenda, feature requests, distractions) while preserving application walkthrough and domain knowledge verbatim. Produces a redacted material file for traceability.
user-invocable: true
allowed-tools: Read, Write, Bash(mkdir*)
argument-hint: [transcript-path]
---

You are sanitising a Defra LAP stakeholder interview transcript. Your job is to remove off-topic material while preserving application walkthrough content and domain knowledge **word-for-word**. You must also capture all removed material in a separate redacted file for traceability.

## Input

The transcript file path is: `$ARGUMENTS`

## Steps

1. **Read the transcript** using the Read tool. The path is relative to the project root.

2. **Analyse the content** and classify every passage into one of two categories:

   **Keep** — retain these passages exactly as written:
   - Application walkthrough content: screen descriptions, user flows, functionality explanations, how the system behaves
   - Domain knowledge: business rules, terminology, processes, regulations, organisational context
   - Any context that helps understand what the application does and why it exists

   **Remove** — cut these passages from the sanitised transcript but capture them in the redacted material file:
   - The incumbent modernisation team's agenda, plans, opinions, or approach discussions
   - Stakeholder feature requests or wishlists for new functionality
   - Tangential conversation not related to the current application or its domain
   - Meta-discussion about the interview process itself (scheduling, logistics, introductions unrelated to the application)

3. **Produce the sanitised transcript** following these rules strictly:
   - Retain kept text **word-for-word** — do not paraphrase, summarise, or rewrite any kept content
   - Where a passage has been removed, insert a `[REDACTED]` marker in its place. The marker must be preceded and followed by a blank line
   - Preserve the original markdown formatting, heading structure, speaker labels, and document layout

4. **Produce the redacted material file** following these rules:
   - List each removed passage **in the order it appeared** in the original transcript
   - Preserve the original text of each passage word-for-word
   - Annotate each passage with the reason for removal using a bold label before the passage text, e.g.:
     - **Reason: Incumbent team agenda**
     - **Reason: Feature request**
     - **Reason: Tangential conversation**
     - **Reason: Interview meta-discussion**
   - Separate each entry with a horizontal rule (`---`)
   - If nothing was removed, do not create the redacted material file

5. **Derive the output paths:**
   - **Sanitised transcript:** insert `sanitised/` after `transcripts/` in the input path. For example:
     - `transcripts/interview-1.md` → `transcripts/sanitised/interview-1.md`
     - `transcripts/sub/deep-dive.md` → `transcripts/sanitised/sub/deep-dive.md`
   - **Redacted material:** insert `redacted/` after `transcripts/` in the input path. For example:
     - `transcripts/interview-1.md` → `transcripts/redacted/interview-1.md`
     - `transcripts/sub/deep-dive.md` → `transcripts/redacted/sub/deep-dive.md`

6. **Ensure the output directories exist** by running `mkdir -p` on the parent directories of both output paths.

7. **Write both files** using the Write tool.

8. **Return a confirmation message** containing:
   - Both output file paths
   - A brief summary of what categories of content were removed, with approximate counts (e.g. "Removed: 3 passages discussing the incumbent team's migration timeline, 2 feature requests for a new reporting dashboard")
   - If nothing was removed, state that the transcript contained no off-topic material and that no redacted material file was created
