# Repository Guidelines

## Purpose

This repository is a personal Obsidian knowledge base designed to grow with its owner. It is not only an archive of notes. It should continuously turn sources, questions, projects, mistakes, and experience into a better connected model of the owner's current understanding.

The operating loop is:

```text
INGEST -> INTEGRATE -> QUERY -> APPLY / REFLECT -> UPDATE -> LINT
```

Prefer cumulative knowledge over disposable summaries. Preserve provenance, synthesize reusable ideas, connect related concepts, and improve existing knowledge before creating duplicates.

## Knowledge Layers

Each directory has a distinct role:

- `1 - Journals/`: chronological personal context, reflection, planning, and lived experience.
- `2 - Source Materials/`: preserved external inputs. This answers: **What did the source say?**
- `3 - Tags/`: topic nodes and lightweight maps connecting related Full Notes.
- `4 - Indexes/`: high-level domain maps for navigating the knowledge base.
- `5 - Templates/`: note templates. `Raw Note.md` is the user's minimal manual template; `AI Raw Note.md` is the structured template for AI-assisted ingestion.
- `6 - Full Notes/`: canonical synthesized knowledge. This answers: **What do I currently understand?**
- `Attachments/`: images and other embedded assets.

Git history records how the knowledge base changed over time.

## Core Principles

### Preserve Sources

Do not replace source material with synthesis. External material belongs in `2 - Source Materials/` so its provenance remains available.

### Compile Knowledge

The goal of ingestion is not merely to create one source note. Identify reusable knowledge and integrate it into the existing wiki when appropriate.

One source may contribute to several Full Notes. One Full Note may synthesize many sources.

### Search Before Create

Before creating a Full Note:

1. Identify the underlying concept.
2. Search `6 - Full Notes/` for the concept and common aliases.
3. Search related Tags and Indexes for existing terminology.
4. If a suitable canonical note exists, update it rather than creating another note.
5. Create a new Full Note only when the concept has a distinct, reusable identity.

Avoid variants such as `Topic New.md`, `Topic 2.md`, or source-specific Full Notes when an existing canonical concept can be improved instead.

### Prefer Integration Over Duplication

New material should normally enrich existing knowledge by adding:

- a clearer explanation;
- an important distinction;
- evidence or examples;
- trade-offs or limitations;
- a meaningful relationship;
- a correction;
- practical experience;
- unanswered questions.

Do not add information merely to make a note longer.

### Full Notes Are Living Documents

A Full Note represents the best current synthesized understanding of a reusable concept, not what was learned from one source on one date.

When new evidence or experience changes the understanding:

- update the canonical Full Note;
- keep useful prior content unless it is wrong or superseded;
- preserve references;
- record nuance when sources disagree instead of hiding disagreement.

Git history preserves prior versions.

### Human Judgment Is Authoritative

AI may organize, synthesize, connect, and propose changes, but must not fabricate personal experience, opinions, decisions, or reflections.

Sections such as `My Experience`, personal conclusions, project lessons, and changes of mind must be grounded in user-provided information, journals, or documented project history.

## Source Material Ingestion

When the user asks to collect, ingest, save, archive, or add external material:

- Always place the source note under `2 - Source Materials/`.
- Choose the most appropriate existing subfolder, such as `Articles/`, `Books/`, `Course/`, `Videos/`, or `Social Media Posts/`.
- Prefer an existing subfolder.
- If no existing subfolder is a reasonable fit, ask whether a new one should be created.
- Do not place source material directly at the repository root.
- For AI-assisted ingestion, follow `5 - Templates/AI Raw Note.md`.
- Keep `5 - Templates/Raw Note.md` as the user's minimal manual raw-note template; do not overwrite or expand it to enforce AI ingestion structure.
- Do not add YAML frontmatter unless the template is changed to use it.
- Preserve the original source URL in `# References`.
- Capture enough substance that the note remains useful without reopening the source.
- Favor faithful synthesis over aggressive compression.

### Required Source-Material Body

Unless the source is genuinely too small, include:

1. `## Headline`
   - concise central message or argument.

2. `## Summary`
   - substantive synthesis of the source, its context, and why it matters.

3. Descriptive `##` content sections
   - primarily point-form;
   - preserve workflows, examples, numbers, tools, distinctions, arguments, or steps when relevant;
   - use source-specific headings rather than generic labels.

4. `## Key Ideas`
   - reusable concepts and takeaways in point form.

5. `## Remarks`
   - end with exactly: `Co-authored by ChatGPT`.

6. `# References`
   - final top-level section;
   - include the original source and useful canonical/supporting links when relevant.

## Knowledge Integration

After ingesting a meaningful source, evaluate whether it adds durable knowledge.

### Integrate When the Material Contains

- a reusable concept;
- an important correction;
- a meaningful relationship between concepts;
- a durable principle or mental model;
- a practical technique likely to be reused;
- an important trade-off or limitation;
- a lesson that changes an existing understanding.

### Do Not Integrate by Default When It Is

- a one-off command;
- a transient status update;
- a simple calculation;
- temporary travel or pricing information;
- a disposable output with no reusable lesson.

### Integration Workflow

1. Identify reusable ideas from the source.
2. Search existing Full Notes before creating anything.
3. Update the most appropriate canonical note(s).
4. Create a new Full Note only when needed.
5. Link relevant Tags.
6. Update a domain Index when navigation meaningfully improves.
7. Add supporting Source Materials under `# References`.
8. Avoid copying source wording when synthesis is possible.

## Full Notes

Full Notes are the canonical knowledge layer.

A Full Note should generally represent one stable concept, technique, mental model, system, or principle that is useful independently of a single source.

Use whatever sections best fit the concept. Common sections include:

- `## Core Idea`
- `## How It Works`
- `## Why It Matters`
- `## Practical Use`
- `## Trade-offs / Limitations`
- `## Examples`
- `## My Experience`
- `## Questions / Gaps`
- `## Related Concepts`
- `# References`

Do not force every note to contain every section.

### Full Note Quality

Prefer notes that:

- explain ideas in the owner's own conceptual model rather than mirroring one source;
- distinguish similar concepts;
- include useful counterexamples and limitations;
- connect to related Full Notes with wikilinks;
- link upward to relevant Tags;
- cite supporting Source Materials;
- become clearer as new knowledge arrives.

## Tags

The preferred navigation hierarchy is:

```text
Index -> Tag -> Full Notes
```

Indexes should guide navigation into Tags. Mature Tags act as curated topic maps that expose relevant Full Notes. Do not make Indexes bypass the Tag layer by turning them into direct catalogs of Full Notes.

Tag files can exist in two states.

### Lightweight Tag

An empty or minimal file used primarily as a backlink target. This is acceptable for small topics.

### Topic Map

Promote an important Tag into a topic map when the topic has accumulated meaningful sub-concepts or navigation is becoming difficult.

A topic map may contain:

- a short overview;
- `## Core Concepts` with links to Full Notes;
- `## Related Topics`;
- optional important Sources.

Do not populate every empty Tag merely for completeness.

## Indexes

Indexes are high-level domain maps, not exhaustive note lists.

Each Index should help a person quickly understand:

- what the domain contains;
- its major topic areas;
- where to enter the knowledge graph.

Prefer grouped semantic sections over flat lists when a domain grows.

Update an Index when a new Tag or domain grouping materially changes navigation. Do not update it for every Full Note.

Prefer Index links to `3 - Tags/` topic nodes rather than direct links to individual Full Notes.

## Query Workflow

Questions can improve the knowledge base.

When answering a substantial question about an area covered by the vault:

1. Search existing Full Notes, Tags, Indexes, Journals, and Source Materials as appropriate.
2. Answer using the best available knowledge.
3. Identify whether the reasoning produced a durable new insight, correction, connection, or practical lesson.
4. If durable knowledge was created and the user wants it integrated, update the relevant canonical Full Notes rather than storing the whole conversation.
5. Add or update links only where they improve future retrieval.

Do not automatically save every chat answer.

## Experience and Reflection

Experience is a first-class input to learning.

When the user provides a lesson from work, a project, a mistake, or an experiment:

- preserve chronological context in Journals when appropriate;
- extract the reusable lesson into relevant Full Notes;
- distinguish generalized knowledge from one specific incident;
- preserve the user's wording and judgment when it matters;
- never invent personal experience.

The preferred loop is:

```text
Learn -> Understand -> Apply -> Observe -> Reflect -> Update
```

## Knowledge Lint

A knowledge-lint pass should inspect the vault and report opportunities such as:

### Structure
- broken wikilinks;
- duplicate or near-duplicate concepts;
- orphan Full Notes;
- orphan Source Materials.

### Knowledge
- Full Notes with weak or missing references;
- recurring concepts without canonical Full Notes;
- contradictions or outdated explanations;
- repeated knowledge spread across several notes.

### Navigation
- important Tags that should become topic maps;
- domains that need stronger Index coverage;
- Full Notes missing useful Tag links.

### Source Health
- source notes missing URLs;
- older source notes that do not follow the current ingestion structure.

### Quality
- shallow Full Notes;
- notes that mostly copy a single source;
- stale concepts worth revisiting.

By default, lint should report findings before making broad or destructive edits. Prefer progressive enrichment over bulk migration.

## Progressive Enrichment

Do not rewrite the entire historical vault merely to satisfy newer conventions.

Upgrade older notes when they become relevant again:

```text
old note -> reused -> reviewed -> improved -> reconnected
```

This keeps maintenance proportional to actual value.

## Development & Validation

Open this directory as a vault in Obsidian to edit and preview content. There is no project build command or automated test suite.

Useful PowerShell commands:

- `rg --files -g '*.md'`: list Markdown notes.
- `rg -n -F '[[Note Title]]' --glob '*.md'`: find references before changing a note.

Validate edits in Obsidian: check heading structure, wikilinks, image embeds, and fenced code blocks.

## Markdown Style & Naming

Use descriptive filenames matching the note topic; preserve existing capitalization and multilingual titles.

- Use `[[Topic]]` for ordinary concept-to-concept links.
- In a Full Note's `Tags:` line, use explicit Tag paths in the form `[[3 - Tags/tag name|tag name]]` so Tags cannot be confused with Full Notes of similar names.
- When an existing Full Note is edited for another reason, normalize its `Tags:` links to this explicit form. Do not bulk-rewrite untouched historical notes only for link style.
- Use `![[filename.png]]` for image embeds.
- Label code fences with their language when practical.
- Keep nested-list indentation consistent with nearby notes.
- Restrict formatting changes to edited content.

## Content Care

Preserve attachment names and update incoming links when moving or renaming notes. Avoid incidental changes to `.obsidian/workspace.json`.

Keep credentials, secrets, and sensitive personal material out of public/shared contributions.
