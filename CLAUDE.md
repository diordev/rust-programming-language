# Rust Programming Language LLM Wiki

## Project Overview

This is a personal LLM-maintained wiki for learning Rust and keeping a durable Rust reference. The user curates sources and asks questions; the LLM reads sources, writes wiki pages, updates links, and keeps the structure coherent.

The working language is Uzbek for explanations, summaries, and notes. Keep Rust terms in English when they are standard technical terms: `ownership`, `borrowing`, `lifetimes`, `traits`, `generics`, `unsafe`, `crate`, `module`, `pattern matching`, and similar.

## Directory Structure

```text
raw/
  books/          # Book chapters and chapter notes in markdown/text
  notes/          # Personal notes
  articles/       # Web articles and blog posts
  youtube/        # Video or podcast transcripts
  papers/         # Papers converted to markdown
  assets/         # Original PDFs, images, binaries

wiki/
  index.md        # Catalog of all wiki content
  overview.md     # Current synthesis of the whole wiki
  log.md          # Append-only operation log
  sources/        # One summary per raw source
  chapters/       # Chapter-level learning pages
  concepts/       # Core concepts such as ownership, borrowing, lifetimes
  examples/       # Explained code examples
  exercises/      # Exercises, solutions, and review notes
  questions/      # Saved Q&A and research questions
  patterns/       # Rust idioms and recurring implementation patterns
  comparisons/    # Comparisons such as String vs &str
  glossary/       # Short definitions
  projects/       # Practice project notes
  snippets/       # Reusable code snippets
  errors/         # Compiler errors, causes, fixes
  crates/         # Crate notes
  tools/          # Tooling notes: cargo, rustfmt, clippy, rustup
  roadmap/        # Learning roadmap and progress
  _templates/     # Page templates

infranodus/       # Flat ontology files only
output/           # Reports, graph analyses, exported answers
todos/            # Research and learning workstreams
```

Organize `raw/` by source type, not by topic. Convert PDFs to markdown before placing them in `raw/papers/`; keep originals in `raw/assets/`.

## Core Conventions

- File names use kebab-case, for example `ownership-and-borrowing.md`.
- Use Obsidian-compatible wikilinks: `[[ownership]]`, `[[borrowing]]`.
- Use ISO dates: `YYYY-MM-DD`.
- Prefer concise Uzbek prose with English Rust terms where they are canonical.
- Use code fences with the `rust` info string for Rust examples.
- Do not overwrite user-written raw sources. `raw/` is read-only for processing.
- The LLM owns `wiki/`, `output/`, `infranodus/`, and `todos/`, but preserve user edits when they exist.

## Frontmatter

Use YAML frontmatter for wiki pages:

```yaml
---
title: "Page Title"
type: concept
status: draft
created: 2026-05-06
updated: 2026-05-06
tags: [rust]
source_count: 0
---
```

Allowed `type` values: `source`, `chapter`, `concept`, `example`, `exercise`, `question`, `pattern`, `comparison`, `glossary`, `project`, `snippet`, `error`, `crate`, `tool`, `roadmap`, `overview`.

Allowed `status` values: `draft`, `active`, `needs-review`, `stable`, `superseded`.

## Page Templates

### Source Summary

Use for every processed file from `raw/`.

Sections:

- `# Title`
- `## Source`
- `## Detailed Summary`
- `## Key Concepts`
- `## Code Examples`
- `## Exercises or Practice Ideas`
- `## Questions Raised`
- `## Links To Update`

Summaries should be detailed. Capture important claims, examples, exercises, mistakes, and confusing points. Link concepts with wikilinks.

### Chapter Page

Use when a source maps to a book chapter.

Sections:

- `# Chapter Title`
- `## Learning Goal`
- `## Main Ideas`
- `## Concepts`
- `## Examples`
- `## Exercises`
- `## Review Questions`
- `## Related Pages`

### Concept Page

Use for durable Rust concepts.

Sections:

- `# Concept`
- `## Short Definition`
- `## Why It Matters`
- `## Mental Model`
- `## Syntax and Examples`
- `## Common Mistakes`
- `## Related Concepts`
- `## Sources`

### Error Page

Use for recurring compiler errors.

Sections:

- `# Error or Diagnostic`
- `## Symptom`
- `## Cause`
- `## Fix Pattern`
- `## Minimal Example`
- `## Related Concepts`

### Question Page

Use for saved answers and open questions.

Sections:

- `# Question`
- `## Answer`
- `## Evidence`
- `## Follow-up`
- `## Related Pages`

## Ingest Workflow

When the user asks to ingest a source:

1. Read the selected raw source.
2. Discuss key takeaways with the user before writing final wiki updates, because this wiki uses interactive per-source ingest.
3. Create or update one page in `wiki/sources/`.
4. Create or update relevant pages in `wiki/chapters/`, `wiki/concepts/`, `wiki/examples/`, `wiki/exercises/`, `wiki/questions/`, `wiki/errors/`, and other folders as needed.
5. Update `wiki/index.md`.
6. Update `wiki/overview.md` if the source changes the bigger picture.
7. Append to `wiki/log.md`.
8. Flag contradictions, unclear claims, and topics that need review.

Good answers in conversation should be saved automatically into `wiki/questions/` or the most relevant existing page. Ask first only when the answer would create a new high-level convention, rename a page family, or substantially reorganize the wiki.

## Process Raw To Wiki

For batch processing, ingest everything in `raw/` that does not yet have a matching `wiki/sources/*.md` page. The matching slug comes from the raw file stem in kebab-case.

Before processing, report the unprocessed files and ask the user to confirm scope:

- all `raw/`
- one subfolder
- one file

## Query Workflow

When answering a Rust question against the wiki:

1. Read `wiki/index.md`.
2. Read relevant concept, chapter, source, and question pages.
3. Answer in Uzbek, keeping Rust terms in English.
4. Cite wiki pages with wikilinks.
5. If the answer is useful and durable, save it automatically as a question page or update an existing page.

## Lint Workflow

Periodic lint checks should:
- Find orphan pages.
- Find missing wikilinks.
- Find concepts mentioned repeatedly but lacking pages.
- Find stale or contradictory claims.
- Find source summaries not linked from `wiki/index.md`.
- Find code examples that should become snippets.
- Suggest new exercises and review questions.
- Checks `[[link]]` on pages — does the linked page actually exist?
- Is this page **not linked from anywhere**? Orphan pages — pages that have been separated from the wiki, and no one can find them.
- Does each page have a front-matter and is it correct? Required fields: `title`, `status`, `created`, `updated`.
- There is a file in `raw/`, but there is no summary page in `wiki/sources/`.

Minor fixes can be handled automatically. Ask before major restructuring.

## Ontology And InfraNodus Workflow

All ontology files live flat in `infranodus/`.

After creating or significantly updating pages in a wiki folder:

1. Read the existing ontology file first, if it exists.
2. Append only genuinely new relations.
3. Never regenerate an ontology from scratch.
4. Match the existing relation style exactly.
5. Save analysis reports in `output/`.

Use wikilink relation syntax, for example:

```text
[[ownership]] [prevents] [[data races]]
[[borrowing]] [enables] [[references without ownership transfer]]
```

If the ontology-creator skill or InfraNodus tools are unavailable, update the wiki normally and record the missing graph step in `wiki/log.md`.

## Tooling Notes

Recommended Obsidian plugins:

- Dataview for querying YAML frontmatter.
- Obsidian Web Clipper for capturing articles.
- Graph View for navigating wikilinks.
- InfraNodus AI Graph View for gap analysis.

Use `rg` for local search. Use git commits after meaningful ingest batches or schema changes.