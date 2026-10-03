# Personal Knowledge Base

This vault is a living learning system.

Its purpose is not to collect as many notes as possible. It should help me remember what matters, understand ideas more deeply, apply them in real situations, reflect on the results, and gradually improve my judgment.

The core loop is:

```text
Learn -> Understand -> Apply -> Reflect -> Improve
```

AI helps maintain the structure, connect knowledge, and reduce repetitive bookkeeping. I remain responsible for curiosity, experience, judgment, and deciding what matters.

## How the Vault Is Organized

### 1 - Journals

Use Journals for chronological context:

- what I am working on;
- what happened;
- what I struggled with;
- what I noticed;
- reflections and changes of mind;
- lessons from projects or real life.

Journals preserve the story.

Reusable lessons should eventually be synthesized into Full Notes.

### 2 - Source Materials

Source Materials preserve what an external source said.

Examples:

- articles;
- books;
- courses;
- videos;
- social media posts.

These notes should keep enough detail and references that I can understand the source later without reopening it.

They are evidence and input, not my final understanding.

### 3 - Tags

Tags connect related knowledge.

A small topic can stay as an empty backlink target.

When a topic grows, its Tag page can become a small topic map containing:

- an overview;
- important Full Notes;
- related topics.

### 4 - Indexes

Indexes are maps of broad domains.

Examples include:

- AI and Agentic Systems;
- Software Engineering;
- Databases and Query Processing;
- Data Engineering and Streaming.

Use them when I want to explore a domain rather than look up one specific concept.

The preferred navigation path is:

```text
Index -> Tag -> Full Notes
```

Indexes guide me to the right topic area. Mature Tag pages then organize the canonical Full Notes inside that topic.

### 5 - Templates

Templates provide starting structures for new notes.

### 6 - Full Notes

Full Notes are the most important knowledge layer.

A Full Note means:

> This is my best current synthesized understanding of this concept.

It should not simply repeat one source.

A Full Note can combine:

- several sources;
- explanations from conversations;
- examples;
- project experience;
- mistakes;
- trade-offs;
- later corrections.

When my understanding changes, update the same canonical Full Note rather than creating `Topic 2` or `Topic New`.

Git history preserves earlier versions.

## What Should I Do When...

### I Find an Interesting Article, Video, Post, or Book

Give it to the agent and ask:

> Ingest this into my knowledge base.

The normal workflow is:

```text
source
  -> Source Material
  -> identify reusable knowledge
  -> update existing Full Notes
  -> create new Full Notes only when needed
  -> connect Tags / Indexes when useful
```

I usually should not need to decide the folder, filename, or note structure manually.

### I Learn Something Important at Work

Tell the agent what happened and what I learned.

For example:

> I learned that reusing these Spark DataFrames caused the logical plan to blow up, and localCheckpoint fixed it by cutting lineage. Integrate this lesson into my KB.

The agent should separate:

- the specific incident;
- the reusable technical lesson.

The incident can remain in a Journal or project context.

The durable lesson belongs in relevant Full Notes.

### I Ask ChatGPT a Useful Question

Most chat answers do not need to become notes.

But when a conversation produces a durable insight, ask:

> Integrate the reusable knowledge from this conversation into my KB.

Good candidates include:

- a concept I finally understood;
- an important correction;
- a relationship between ideas;
- a reusable troubleshooting lesson;
- a principle I expect to apply again.

Do not save disposable answers merely because they exist.

### I Discover That I Was Wrong

Update the canonical Full Note.

Do not preserve a wrong statement merely because it used to be my belief.

If the change itself is meaningful, capture the nuance:

- what I previously assumed;
- what evidence changed the conclusion;
- when the old model is still useful;
- what the better model is now.

Git already preserves historical versions.

### I Want to Learn a Topic

Useful prompts include:

> Teach me X using my knowledge base.

> What do I already know about X?

> What am I missing about X based on my KB?

> Build a learning path from what I already understand.

> Explain X by connecting it to concepts I already know.

The aim is to build on existing understanding rather than restart from generic explanations.

### I Want to Decide What to Learn Next

Ask for a knowledge-gap review.

Examples:

> Review my KB and find areas where I have many sources but weak understanding.

> Which concepts do I keep encountering but have not applied?

> Which areas are becoming strengths, and where are the biggest gaps?

> What would be the highest-leverage next topic based on my current notes and projects?

## How to Use Full Notes

A useful Full Note should become more valuable over time.

Common sections can include:

```markdown
# Concept

## Core Idea

## How It Works

## Why It Matters

## Practical Use

## Trade-offs / Limitations

## Examples

## My Experience

## Questions / Gaps

## Related Concepts

# References
```

Not every note needs all of these.

A mature Full Note should answer more than "what is this?"

It should increasingly help me answer:

- when should I use this?
- when should I not use it?
- what is it often confused with?
- what did I learn by applying it?
- what assumptions have changed?
- what remains unclear?

## Turning Knowledge Into Growth

The knowledge base is useful only if information moves beyond recognition.

A helpful mental model is:

```text
Knowledge
= I understand the idea.

Skill
= I can use it.

Experience
= I have used it in real situations.

Judgment
= I know when to use it and when not to.
```

When learning something important, try to move it through those stages.

For example, after learning a design principle:

1. understand the definition;
2. identify it in real code;
3. apply it in a project;
4. observe where it helped or hurt;
5. update the Full Note with that experience.

## How to Use Journals for Learning

Journals are not another encyclopedia.

Use them to preserve context such as:

```markdown
## Learning

Today I finally understood why the Spark driver stalled.

Related:
- [[Spark Logical Plans]]
- [[Spark DataFrame Lineage]]

What surprised me:
- ...

What changed in my understanding:
- ...
```

Later, reusable knowledge can be integrated into Full Notes.

Think of it as:

```text
Journal = context and history
Full Note = generalized understanding
```

## Working With AI

The AI should handle much of the maintenance work:

- choose source folders;
- format ingested material;
- search before creating notes;
- find related concepts;
- update canonical Full Notes;
- propose links;
- improve topic maps;
- inspect the KB for gaps and duplicates.

I should focus more on:

- choosing what is worth learning;
- asking good questions;
- applying knowledge;
- noticing surprising outcomes;
- challenging explanations;
- reflecting on experience.

The goal is not for the AI to build an encyclopedia that I never internalize.

The goal is for the KB and AI to help me think better.

## Knowledge Health Checks

Periodically ask:

> Run a knowledge-base health check.

Useful checks include:

- broken links;
- duplicate concepts;
- orphan Full Notes;
- Source Materials that never contributed to reusable knowledge;
- Full Notes with weak references;
- frequently mentioned concepts without canonical notes;
- important empty Tags that should become topic maps;
- areas with many notes but weak Index navigation;
- contradictory explanations;
- old knowledge that may need revisiting.

Health checks should normally report first instead of rewriting everything automatically.

## Do Not Bulk-Clean the Vault

Older notes do not all need to be rewritten immediately.

Use progressive enrichment:

```text
old knowledge
  -> becomes relevant
  -> review it
  -> improve it
  -> connect it
```

This keeps effort focused on knowledge I actually use.

## A Simple Weekly Practice

I do not need a complicated review ritual.

Once a week, I can ask:

- What did I learn that was genuinely new?
- What did I apply?
- Did any experience challenge an existing belief?
- Is there anything worth integrating into Full Notes?

## A Deeper Monthly Review

A useful monthly prompt is:

> Review my knowledge base and tell me:
>
> 1. What did I meaningfully learn this month?
> 2. What did I actually apply?
> 3. Where did my understanding change?
> 4. What topics am I consuming without applying?
> 5. What recurring mistakes or difficulties appeared?
> 6. What areas are becoming strengths?
> 7. What important gaps should I work on next?

The purpose is not to produce a score.

It is to make my learning visible and help me choose the next useful direction.

## The Desired Long-Term Outcome

After years of use, the KB should be able to answer two different questions:

> What do I know about this topic?

and, more importantly:

> How has my understanding of this topic changed, and what have I learned from applying it?

That is the standard for whether this system is actually growing with me.
