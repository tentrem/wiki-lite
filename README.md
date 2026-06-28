# wiki-lite

Lightweight **LLM Wiki + qmd** template for project knowledge bases.

## Layout

```text
AGENTS.md          # agent policy and retrieval flow
raw/               # source of truth
wiki/              # LLM-written synthesis
.qmd/              # qmd local index/cache
.agents/skills/    # project-local skills
```

## Install

```bash
npm install -g @tobilu/qmd
qmd doctor
```

## Bootstrap

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

## Query

```text
question → wiki/ → raw/ → resources → not found
```

```bash
qmd search "exact terms" -n 5
qmd vsearch "semantic question" -n 5
qmd query "hybrid question" -n 5
qmd get "path:start:count"
```

## Ingest

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
