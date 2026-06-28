---
name: llm-wiki
description: >
  Use for projects that maintain an LLM-generated wiki over raw source
  documents, especially when the project has AGENTS.md, wiki/, raw/, and/or
  qmd. Handles all agent interaction with wiki data: retrieval, answering with
  citations, ingesting sources, updating wiki pages, logging, linting, and
  re-indexing. Trigger when the user says llm-wiki, wiki ingest, update wiki,
  query wiki, lint wiki, qmd wiki, or asks about project knowledge stored in
  raw/wiki folders.
argument-hint: "[query|ingest|lint|status]"
license: MIT
---

# LLM Wiki

Operate as the wiki maintainer for a project with this shape:

```text
project/
  AGENTS.md        # policy, source list, retrieval rules
  raw/             # source of truth; do not edit except file moves/renames requested by user
  wiki/            # LLM-written synthesis; edit this
  .qmd/            # local qmd index/cache; do not commit DB/cache
```

## Core rule

Raw sources are truth. Wiki is compiled memory. No source = no fact.

Always prefer the project's `AGENTS.md` if present. It overrides this skill for
project-specific paths, language, and source priority.

## Query flow

When answering a domain question:

1. Read `AGENTS.md` if present.
2. Search `wiki/` first.
3. If wiki evidence is enough, answer from wiki and cite raw sources listed in the page.
4. If weak/missing, search raw sources.
5. Use large/full-text files only as fallback.
6. If still missing, say not found and list searched sources.
7. If the answer is reusable, update `wiki/`, append `wiki/log.md`, then re-index.

Use qmd from the project root when `.qmd/` exists:

```bash
qmd search "exact terms" -n 5
qmd vsearch "semantic question" -n 5
qmd query "hybrid question" -n 5
qmd get "path:start:count"
```

Lazy default: `qmd search` first. Use `vsearch/query` only when exact search is not enough.

## Ingest flow

Do not brute-force read giant documents. Ingest one reusable wiki target at a time.

1. Identify the source or topic.
2. Search headings/snippets with `qmd search` or `rg`.
3. Read only relevant slices with `qmd get "file:start:count"`.
4. Create/update one wiki page.
5. Add raw-source citations for every important fact.
6. Update `wiki/index.md` if page added/renamed.
7. Append `wiki/log.md`.
8. Run `qmd update`; run `qmd embed` after a batch or when semantic search quality matters.

## Wiki page format

```markdown
# Title

## Summary
Short synthesis.

## Key facts
- Fact. Source: `raw/path.md#heading`

## How to use
Practical guidance.

## Source evidence
- `raw/path.md` — heading/section

## Open questions
- Unknown / needs verification.
```

Move unsupported claims to `Open questions`.

## Log format

Append only:

```markdown
## [YYYY-MM-DD] ingest | topic
- Updated pages: `wiki/...`
- Sources: `raw/...`
```

Use `query`, `ingest`, `lint`, or `maintenance` as the action.

## Lint flow

Periodically check:

- pages missing `Source evidence`
- orphan pages absent from `wiki/index.md`
- duplicate/overlapping pages
- claims without raw citations
- contradictions against raw sources
- pages that are too long and should point back to raw source instead

Fix by deleting/merging before adding new pages.

## Output

For answers, include a short `Sources:` list.
For maintenance, report changed files and re-index status.
