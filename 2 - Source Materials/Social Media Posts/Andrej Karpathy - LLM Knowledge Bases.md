2026-04-03 04:42

# LLM Knowledge Bases

## Headline

LLMs can be used as knowledge compilers: collect raw source material, let the model continuously organize it into an interlinked Markdown wiki, and make every future query improve the knowledge base instead of disappearing into a one-off chat.

## Summary

Andrej Karpathy describes a personal research workflow in which a growing share of his LLM usage has shifted from manipulating code to manipulating knowledge stored as Markdown and images. Rather than repeatedly feeding raw articles, papers, repositories, datasets, and images into isolated conversations, he keeps the source material locally and has an LLM incrementally compile it into a structured wiki.

The wiki is not treated as a passive archive. The LLM maintains summaries, indexes, backlinks, concept pages, and relationships across sources. Obsidian acts as the human-facing interface for browsing the raw inputs, the compiled knowledge, and derived outputs such as slides and visualizations.

Once the wiki becomes sufficiently rich, it becomes a substrate for research and Q&A. The answers and artifacts generated from those queries can then be filed back into the wiki, so repeated exploration compounds rather than resets. Karpathy also uses LLM-based health checks to detect inconsistencies, fill missing information, discover connections, and suggest new research directions.

## Data Ingest

- Collect heterogeneous source material such as:
  - articles
  - research papers
  - code repositories
  - datasets
  - images
- Store the original material in a local `raw/` directory.
- Use an LLM to incrementally compile the raw material into a directory of Markdown files.
- The compiled wiki should contain more than summaries:
  - backlinks between related notes
  - concept groupings
  - synthesized articles
  - links across related ideas and sources
- For web content, Karpathy mentions using the Obsidian Web Clipper to convert pages into Markdown.
- Related images are downloaded locally so that the LLM can reference them together with the text.

## Obsidian as the Knowledge IDE

- Obsidian is used as the front end for the complete knowledge workflow.
- It provides one place to inspect:
  - original raw material
  - LLM-compiled wiki pages
  - generated visualizations
  - other derived artifacts
- The important ownership model is that the LLM writes and maintains the wiki.
- The human primarily explores, queries, and reviews it rather than manually maintaining every page.
- Obsidian plugins can extend the presentation layer, such as rendering slide decks with Marp.

## Querying the Wiki

- The workflow becomes more valuable once enough related knowledge has accumulated.
- Karpathy gives an example research wiki of roughly:
  - 100 articles
  - 400,000 words
- At that scale, an LLM agent can answer fairly complex questions by navigating the existing wiki.
- He initially expected to need more elaborate RAG infrastructure.
- Instead, at this scale, the LLM can maintain useful index files and short document summaries and use them to locate relevant material.
- This suggests that well-structured, model-maintained Markdown can itself serve as an effective retrieval layer for a moderately sized personal knowledge base.

## Outputs Feed Back Into the Knowledge Base

- Answers do not have to remain temporary terminal or chat output.
- Useful outputs can be rendered as:
  - Markdown notes
  - Marp slide decks
  - matplotlib images
  - other query-specific visual formats
- Valuable outputs can be filed back into the wiki.
- This creates a compounding loop:
  - collect source material
  - compile knowledge
  - ask questions
  - generate new synthesis
  - store the synthesis
  - use the enriched wiki for the next question
- The important shift is from disposable AI conversations to cumulative knowledge work.

## Linting and Knowledge Health Checks

- LLMs can perform periodic health checks across the wiki.
- Possible checks include:
  - identifying inconsistent or conflicting information
  - detecting missing data
  - filling gaps with additional web research
  - finding previously unnoticed relationships
  - proposing candidates for new concept articles
- The model can also suggest follow-up questions that may be worth investigating.
- This turns knowledge maintenance into an active process rather than simple storage.

## Extra Tools

- As the wiki grows, custom tools can be built around it.
- Karpathy describes creating a small search engine over the wiki.
- The search tool can be used directly through a web UI.
- More importantly, it can also be exposed to an LLM through a CLI so the agent can use it while answering larger research questions.
- This makes the knowledge base an environment that agents can operate on, not just a collection of documents for humans to browse.

## Further Exploration

- A larger knowledge repository creates opportunities beyond context-window retrieval.
- One possible direction is generating synthetic data from the accumulated knowledge.
- Another is fine-tuning models so some of the domain knowledge becomes represented in model weights rather than always being loaded through context.
- Karpathy also imagines a more agentic version of the workflow where a difficult question could trigger multiple LLMs to:
  - construct a temporary research wiki
  - collect and organize evidence
  - lint and refine the material
  - iterate several times
  - produce a final report
- The broader product opportunity is a system that makes this workflow native instead of relying on a collection of scripts.

## Key Ideas

- Treat LLMs as tools for manipulating and maintaining knowledge, not only code.
- Preserve raw source material and separate it from the model-maintained knowledge layer.
- Let the LLM continuously compile raw information into structured, linked Markdown.
- Use human-readable files so both people and agents can inspect and operate on the same knowledge base.
- A well-maintained local wiki can be sufficient for retrieval at moderate scale without immediately requiring sophisticated RAG infrastructure.
- Store useful AI-generated outputs back into the knowledge base so research compounds over time.
- Use LLMs for knowledge quality control, including consistency checking, gap filling, connection discovery, and research-question generation.
- Obsidian works well as the human-facing IDE, while CLI tools and agents provide the machine-facing interface.
- The long-term direction is a persistent research system in which models continuously ingest, organize, query, verify, and extend knowledge.

## Remarks

Co-authored by ChatGPT

# References

- Andrej Karpathy, "LLM Knowledge Bases": https://x.com/karpathy/status/2039805659525644595
- Thread Reader mirror of the post: https://threadreaderapp.com/thread/2039805659525644595.html
- Karpathy's LLM Wiki gist: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
