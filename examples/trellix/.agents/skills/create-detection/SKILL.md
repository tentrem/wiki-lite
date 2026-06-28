---
name: create-detection
description: >
  Use for creating or updating Trellix ESM detection rules, correlation rules,
  alarm XML, rule documentation, tuning guidance, and wiki playbook entries in
  this Trellix project.
argument-hint: "[rule-name or detection idea]"
license: MIT
---

# Trellix Create Detection

Use this project-local skill from `Trellix/`. Follow `AGENTS.md` and the project-local
`llm-wiki` skill at `.agents/skills/llm-wiki/SKILL.md` for retrieval/wiki behavior.

## Source layout

```text
raw/references/              # Trellix ESM reference docs
raw/detection-rules/         # raw rule markdown docs
raw/detection-rules/xml/     # raw alarm/correlation XML examples
raw/resources/               # full text extraction, PDFs, samples
raw/exports/                 # raw import/export XML/EXP files
wiki/playbooks/create-detection.md
wiki/rules/*.md
```

Raw sources are truth. Wiki is synthesis. No source = no fact.

## Retrieval before creating rules

Use `llm-wiki` retrieval rules: wiki first, raw sources second, full text last.

Run from `Trellix/`:

```bash
qmd search "create detection correlation rule alarm" -n 5
qmd get "wiki/playbooks/create-detection.md"
qmd search "conditionData actionType severity" -n 5
qmd search "similar detection rule threshold tuning" -n 5
rg -n "conditionType|actionType|severity|matchField" raw/detection-rules/xml raw/exports
```

Read only needed slices:

```bash
qmd get "raw/references/Trellix_ESM_Detection_Engineering_Guide.md:437:80"
qmd get "raw/references/Trellix_ESM_Alarm_XML_Reference.md:1:120"
qmd get "raw/detection-rules/Password_Spray_Detection.md:1:120"
```

## Flow

1. Gather requirements:
   - behavior/attack
   - MITRE ATT&CK tactic/technique
   - data sources
   - threshold/window/group-by expectation
   - response actions
2. Pick rule type:
   - direct single event → alarm
   - multi-event pattern → correlation rule on ACE
   - parsing → ASP rule
   - exclusion/filter → filter/watchlist
3. Build correlation logic when needed:
   - name, description, severity
   - time window
   - group-by fields
   - threshold
   - match conditions
4. Build alarm:
   - name `[SEVERITY] - description`
   - condition type: direct/internal/correlation event
   - trigger frequency
   - actions: visual/email/incident/SOAR only if requested
5. Write raw rule doc if requested:
   - `raw/detection-rules/[Rule_Name]_Detection.md`
6. Ask before creating import XML:
   - alarm XML: `raw/detection-rules/xml/[Rule_Name]_Alarm.xml`
   - correlation XML: `raw/detection-rules/xml/[Rule_Name]_Correlation_Rule.xml`
7. Update wiki only if reusable:
   - `wiki/rules/[slug].md`
   - `wiki/index.md`
   - `wiki/log.md`
8. Re-index:

```bash
qmd update
qmd embed
```

## Rule document skeleton

```markdown
# [Rule Name] - Trellix ESM

## Overview

## MITRE ATT&CK Mapping

## Detection Logic
### Correlation Rule
### Pseudocode

## Trellix ESM Configuration
### Step 1: Create Correlation Rule
### Step 2: Create Alarm

## Alarm XML (Import Format)

## Event Fields to Capture

## Tuning Recommendations
### Reduce False Positives
### Threshold Tuning

## Response Actions
### Immediate
### Investigation
### Remediation

## Sample Correlation Event Output

## Related Detection Rules
```

## Severity guide

| Range | Level | Use case |
|---|---|---|
| 80-100 | Critical | ransomware, data exfil, admin compromise |
| 60-79 | High | credential theft, lateral movement, C2 |
| 40-59 | Medium | reconnaissance, policy violation |
| 1-39 | Low | informational, noisy/high-FP detections |

## Quality check

Before final answer/save:

- MITRE mapping present
- detection logic clear
- threshold/window/group-by explained
- false positives and watchlist tuning included
- alarm behavior and response actions documented
- raw source citations included
- user confirmed before overwriting files or creating XML

## Output

Return changed files and sources. Keep XML in raw; do not paste huge XML into wiki.
