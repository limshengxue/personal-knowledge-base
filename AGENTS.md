# Repository Guidelines

## Project Structure & Organization

This repository is an Obsidian knowledge vault containing Markdown notes and image attachments.

- `1 - Journals/`: personal planning and `Weekly Journal/` entries.
- `2 - Source Materials/`: collected or ingested source material. Organize each item into the most appropriate existing subfolder, such as `Articles/`, `Books/`, `Course/`, `Videos/`, or `Social Media Posts/`.
- `3 - Tags/`: topic notes used as wikilink targets, such as `airflow.md`.
- `4 - Indexes/`: reserved for navigation and overview notes.
- `5 - Templates/`: full-note, raw-note, and weekly-journal templates.
- `6 - Full Notes/`: developed topic notes and the configured destination for new notes.
- `.obsidian/`: application settings, plugins, themes, and workspace state. Root-level PNG files are embedded attachments.

## Source Material Ingestion

When the user asks to collect, ingest, save, archive, or add external material to the knowledge base:

- Always place the resulting note under `2 - Source Materials/`.
- Choose the most appropriate existing subfolder based on the source type or content.
- Prefer an existing subfolder over creating a new one.
- If no existing subfolder is a reasonable fit, ask the user whether a new subfolder should be created before creating it.
- Do not place ingested source material directly at the repository root or directly under `2 - Source Materials/` when a suitable subfolder exists.
- Every source-material note must follow the structure of `5 - Templates/Raw Note.md`:
  - first line: date and time
  - title as a level-1 heading
  - note body
  - final `# References` section
- Do not add YAML frontmatter to a source-material note unless the Raw Note template is changed to include it.
- Preserve the original source URL and include it in `# References`.
- Capture enough context and detail that the note remains useful without reopening the original source.

### Required Source-Material Body

The note body must not be an over-simplified summary. Unless the source is genuinely too small to support the structure, every ingested source-material note must contain all of the following:

1. `## Headline`
   - A concise statement of the source's central message or argument.

2. `## Summary`
   - A substantive synthesis of the source.
   - Explain the overall idea, context, and why the material matters.
   - Do not reduce a substantial source to only a few generic sentences.

3. Multiple content sections using descriptive `##` headings.
   - Create as many sections as needed to represent the source faithfully.
   - Each section should primarily use point-form bullets for scanability.
   - Preserve concrete details, workflows, examples, numbers, tools, distinctions, arguments, or steps from the source when relevant.
   - The section names should reflect the actual material rather than generic labels.

4. `## Key Ideas`
   - Distill the most important reusable concepts, principles, or takeaways.
   - Use point-form bullets.

5. `## Remarks`
   - End this section with exactly: `Co-authored by ChatGPT`

6. `# References`
   - Keep this as the final top-level section in accordance with `5 - Templates/Raw Note.md`.
   - Include the original source and useful supporting or canonical links when relevant.

The goal of ingestion is to create a rich, structured source note that preserves the important substance of the material while making it easy to review later in Obsidian. Favor completeness and faithful synthesis over aggressive compression.

## Development & Validation

Open this directory as a vault in Obsidian to edit and preview content. There is no project build command, automated test suite, or coverage requirement.

Useful PowerShell commands:

- `rg --files -g '*.md'`: list Markdown notes.
- `rg -n -F '[[Note Title]]' --glob '*.md'`: find references before changing a note.

Validate edits in Obsidian: check heading structure, wikilinks, image embeds, and fenced code blocks. For template changes, create a temporary note using the template and verify placeholders resolve correctly.

## Markdown Style & Naming

Use descriptive filenames matching the note topic; preserve existing capitalization and multilingual titles. Follow nearby notes and the templates. Source-material notes must follow `5 - Templates/Raw Note.md`. Use `[[Topic]]` for internal links and `![[filename.png]]` for image embeds.

Keep nested-list indentation consistent with the surrounding note; existing outlines commonly use tabs. Label code fences with their language when practical. The vault enables Format with Prettier, Outliner, Code Styler, and Simple Code Formatter plugins; restrict formatting to edited content.

## Commits & Pull Requests

No Git metadata is present in this checkout, so existing commit conventions cannot be established. If contributing through Git, use concise imperative messages, such as `docs: clarify Airflow scheduling notes`. Describe affected notes, link relevant issues, and include screenshots when rendering changes need visual review.

## Configuration & Content Care

Preserve attachment names and update incoming links when moving or renaming notes. Avoid incidental changes to `.obsidian/workspace.json`. Keep credentials and private journal content out of shared contributions.
