---
name: redact-pii
description: Redacts personally identifiable information (PII) from a curated interview transcript, replacing real names, emails, phone numbers, addresses, and other identifying details with realistic fake equivalents.
user-invocable: true
allowed-tools: Read, Edit, Bash(mkdir*;cp*)
argument-hint: [curated-transcript-path]
---

You are redacting PII from a Defra LAP curated interview transcript. Your job is to replace identifying details with realistic fake equivalents. All non-PII text is preserved mechanically — you never rewrite, summarise, or restructure any transcript content.

## Input

The curated transcript file path is: `$ARGUMENTS`

It should be a `*_curated.txt` file produced by the `curate-transcript` skill.

## Steps

1. **Derive the output path** by replacing the `_curated.txt` suffix with `_redacted.txt`. For example:
   - `transcripts/interview-1_curated.txt` → `transcripts/interview-1_redacted.txt`
   - `transcripts/deep-dive_curated.txt` → `transcripts/deep-dive_redacted.txt`

2. **Copy the curated file** to the output path using `cp`. This creates an exact mechanical copy that preserves all text verbatim.

3. **Read the output file** using the Read tool.

4. **Identify PII to replace.** Look for these categories:

   **Replace** — these must be swapped with realistic fakes:
   - **Personal names** — first names, surnames, full names of individuals mentioned in the transcript
   - **Email addresses** — any email address
   - **Phone numbers** — any phone number (landline or mobile)
   - **National Insurance numbers** — UK NI number patterns (e.g. AB 12 34 56 C)
   - **Postal addresses** — street addresses, postcodes, specific location names that identify individuals
   - **Other identifying details** — employee IDs, staff numbers, or any other detail that could identify a specific person

   **Do not replace:**
   - Organisation names (Defra, APHA, RPA, etc.) — these are not PII
   - Role titles and team names — these describe functions, not individuals
   - Application names and system names
   - Domain terminology and business concepts
   - Place names used in a general business context (e.g. region names, office locations as organisational units)

5. **Build a replacement mapping** before making edits. For consistency:
   - Map each real name to a single fake name and use it consistently throughout the file
   - Use culturally plausible fake names (e.g. replace an English name with an English name)
   - Use realistic fake values for emails, phones, etc. (e.g. `jane.smith@example.gov.uk`, `07700 900123`)
   - If the same person is referenced across the transcript (by first name, surname, or full name), all references must map to the same fake identity

6. **Replace each PII instance** using the Edit tool on the output file. For each replacement:
   - Set `old_string` to the **exact text** containing the PII (copy it precisely)
   - Set `new_string` to the same text with the PII swapped for its fake equivalent
   - Use `replace_all: true` when a name or value appears multiple times throughout the file

7. **Return a confirmation message** containing:
   - The output file path
   - A summary of replacements made by category (e.g. "Replaced: 3 personal names, 2 email addresses, 1 phone number")
   - The replacement mapping used (e.g. "John Davies → Robert Hughes, sarah.jones@defra.gov.uk → jane.smith@example.gov.uk")
   - If no PII was found, state that the transcript contained no personally identifiable information
