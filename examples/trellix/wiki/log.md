# Trellix Wiki Log

## [2026-06-28] query | port scan detection
- Updated pages: `wiki/rules/port-scan.md`, `wiki/index.md`
- Sources: `raw/detection-rules/Port_Scan_Detection.md`, `raw/references/Trellix_ESM_Detection_Engineering_Guide.md`, `raw/references/Trellix_ESM_Alarm_Correlation_Guide.md`

## [2026-06-28] ingest | initial qmd + seed wiki
- Created `AGENTS.md`, qmd collection, wiki structure, and seed pages.
- Sources: `raw/references/Trellix_ESM_Detection_Engineering_Guide.md`, `raw/references/Trellix_ESM_Alarm_Correlation_Guide.md`, `raw/references/Trellix_ESM_Alarm_XML_Reference.md`, `raw/references/Trellix_ESM_Correlation_XML_Reference.md`, `raw/detection-rules/Password_Spray_Detection.md`.

## [2026-06-28] maintenance | raw folder normalization
- Moved raw sources into `raw/references/`, `raw/detection-rules/`, `raw/resources/`, and `raw/exports/`.
- Updated `AGENTS.md`, qmd contexts, wiki citations, and implementation plan paths.
- Ran `qmd update` and `qmd embed`.

## [2026-06-28] query | watchlist limits
- Updated pages: `wiki/concepts/watchlists.md`, `wiki/index.md`
- Sources: `raw/resources/trellix_esm_11.7.x_full_text.md`

## [2026-06-28] lint | phase 4 wiki health check
- Checked orphan pages against `wiki/index.md`: none found.
- Checked wiki pages for `Source evidence` and `Open questions`: all content pages have both.
- Checked raw citation paths: no missing raw markdown references found.
- Checked obvious duplicate pages: none found.
- No prune/merge needed; wiki remains small.
