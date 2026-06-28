# wiki-lite

Lightweight **LLM Wiki + qmd** template for project knowledge bases.

## What this gives you

```text
AGENTS.md          # agent policy and retrieval flow
raw/               # source of truth
wiki/              # LLM-written synthesis
.qmd/              # qmd local index/cache
.agents/skills/    # project-local agent skills
```

Rule: **raw is truth, wiki is synthesis, no source = no fact.**

## Install

```bash
npm install -g @tobilu/qmd
qmd doctor
```

## Adopt in a new project

After cloning `wiki-lite`, run this from your target project:

```bash
cd MyProject
mkdir -p raw/{references,detection-rules,resources,exports} wiki .agents/skills
cp /path/to/wiki-lite/AGENTS.md.template AGENTS.md
cp /path/to/wiki-lite/wiki/index.md wiki/index.md
cp /path/to/wiki-lite/wiki/log.md wiki/log.md
cp -r /path/to/wiki-lite/.agents/skills/llm-wiki .agents/skills/
qmd init
qmd collection add . --name myproject --mask "**/*.md"
qmd update
qmd embed
```

Put source documents in:

```text
raw/references/
raw/detection-rules/
raw/resources/
raw/exports/
```

## Agent usage

Tell the agent:

```text
Read AGENTS.md and .agents/skills/llm-wiki/SKILL.md.
Use wiki first, fallback raw, cite sources.
```

Agent flow:

```text
question → read AGENTS.md → read llm-wiki skill → search wiki/ → search raw/ → answer with citations → update wiki if reusable
```

If evidence is missing, the agent must say `not found` and list searched sources.

## Query commands

```bash
qmd search "exact terms" -n 5
qmd vsearch "semantic question" -n 5
qmd query "hybrid question" -n 5
qmd get "path:start:count"
```

## Ingest flow

1. Pick one target wiki page/topic.
2. Search raw/wiki with qmd or `rg`.
3. Read only relevant slices.
4. Update one wiki page.
5. Cite raw sources.
6. Update `wiki/index.md` and append `wiki/log.md`.
7. Run `qmd update`; run `qmd embed` after a batch.

Windows fallback if `rg` is unavailable:

```powershell
Select-String -Path .\raw\**\*.md,.\wiki\**\*.md -Pattern "keyword"
```

## Git policy

Commit:

```text
AGENTS.md
wiki/**/*.md
.qmd/index.yml
raw/**/.gitkeep
.agents/skills/**/SKILL.md
```

Do not commit:

```text
.qmd/index.sqlite
.qmd/*.sqlite
.qmd/cache/
private raw sources
client secrets
```
