# wiki-lite

A lightweight **LLM Wiki + qmd** template for project knowledge bases.

## Layout

```text
AGENTS.md          # project policy and retrieval flow
raw/               # source of truth; keep original documents here
wiki/              # LLM-written synthesis
.qmd/              # qmd local index/cache
.agents/skills/    # project-local agent skills
```

## Install

Requires Node.js >= 22 and qmd:

```bash
npm install -g @tobilu/qmd
qmd doctor
```

## Bootstrap a new project

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

Optional Pi global skill copy:

```bash
mkdir -p ~/.pi/agent/skills
cp -r .agents/skills/llm-wiki ~/.pi/agent/skills/
```

Project-local skill remains the source of truth.

## Query flow

```text
question → wiki/ → raw/ → full text/resources → not found
```

Use:

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

## Windows search fallback

If `rg` is not installed:

```powershell
Select-String -Path .\raw\**\*.md,.\wiki\**\*.md -Pattern "keyword"
Select-String -Path .\raw\**\*.xml -Pattern "conditionType|actionType|severity|matchField"
```

## Git policy

Commit if shareable:

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
