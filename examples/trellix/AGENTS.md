# Agent Instructions

Use the project-local `llm-wiki` skill at `.agents/skills/llm-wiki/SKILL.md` for all Trellix wiki/raw/qmd interactions.
Use the project-local `create-detection` skill at `.agents/skills/create-detection/SKILL.md` when creating Trellix detection rules.

## Policy
- No hallucination.
- Always cite source evidence.
- Use English for Trellix responses.
- If evidence is missing, say not found and list searched sources.

## Reference Files
- `raw/references/Trellix_ESM_Knowledge_Base.md`
- `raw/references/Trellix_ESM_Detection_Engineering_Guide.md`
- `raw/references/Trellix_ESM_Alarm_Correlation_Guide.md`
- `raw/references/Trellix_ESM_Alarm_XML_Reference.md`
- `raw/references/Trellix_ESM_Correlation_XML_Reference.md`
- `raw/detection-rules/`
- `raw/resources/trellix_esm_11.7.x_full_text.md`

## Retrieval
The AI agent must run retrieval before answering Trellix questions.

Commands:
- exact: `qmd search`
- semantic: `qmd vsearch`
- hybrid: `qmd query`
- read snippets: `qmd get "path:start:count"`

Search order:
1. `wiki/`
2. Reference Files
3. `raw/resources/trellix_esm_11.7.x_full_text.md`
4. official Trellix web sources
5. not found

Answer flow:
1. Search `wiki/` first.
2. If wiki evidence is enough, answer from wiki and cite raw sources listed there.
3. If evidence is weak/missing, search `raw/references/` and `raw/detection-rules/`.
4. Use `raw/resources/trellix_esm_11.7.x_full_text.md` only as fallback.
5. If still missing, say not found and list searched sources.
6. If the answer is reusable, update `wiki/`, append `wiki/log.md`, then run `qmd update`.
