# Repository Guidelines

## Project Structure & Organization

This repository is an Obsidian knowledge vault containing Markdown notes and image attachments.

- `1 - Journals/`: personal planning and `Weekly Journal/` entries.
- `2 - Source Materials/`: notes organized under `Articles/`, `Books/`, `Course/`, and `Videos/`.
- `3 - Tags/`: topic notes used as wikilink targets, such as `airflow.md`.
- `4 - Indexes/`: reserved for navigation and overview notes.
- `5 - Templates/`: full-note, raw-note, and weekly-journal templates.
- `6 - Full Notes/`: developed topic notes and the configured destination for new notes.
- `.obsidian/`: application settings, plugins, themes, and workspace state. Root-level PNG files are embedded attachments.

## Development & Validation

Open this directory as a vault in Obsidian to edit and preview content. There is no project build command, automated test suite, or coverage requirement.

Useful PowerShell commands:

- `rg --files -g '*.md'`: list Markdown notes.
- `rg -n -F '[[Note Title]]' --glob '*.md'`: find references before changing a note.

Validate edits in Obsidian: check heading structure, wikilinks, image embeds, and fenced code blocks. For template changes, create a temporary note using the template and verify placeholders resolve correctly.

## Markdown Style & Naming

Use descriptive filenames matching the note topic; preserve existing capitalization and multilingual titles. Follow nearby notes and the templates: full notes begin with a timestamp, a `Tags:` line, a title heading, and a `References` section. Use `[[Topic]]` for internal links and `![[filename.png]]` for image embeds.

Keep nested-list indentation consistent with the surrounding note; existing outlines commonly use tabs. Label code fences with their language when practical. The vault enables Format with Prettier, Outliner, Code Styler, and Simple Code Formatter plugins; restrict formatting to edited content.

## Commits & Pull Requests

No Git metadata is present in this checkout, so existing commit conventions cannot be established. If contributing through Git, use concise imperative messages, such as `docs: clarify Airflow scheduling notes`. Describe affected notes, link relevant issues, and include screenshots when rendering changes need visual review.

## Configuration & Content Care

Preserve attachment names and update incoming links when moving or renaming notes. Avoid incidental changes to `.obsidian/workspace.json`. Keep credentials and private journal content out of shared contributions.
