# Codex Compatibility

Career-Ops is not Claude-only at the data and script layer. It is Claude-specific at the agent UX layer.

## Current Status

Works well in Codex:
- Markdown/YAML data model
- Evaluation/report workflow
- PDF generation scripts
- Tracker integrity scripts
- Dashboard TUI
- Template and profile customization

Needs adaptation in Codex:
- `/career-ops` slash commands
- `.claude/skills/career-ops/SKILL.md`
- `batch/batch-runner.sh` because it depends on `claude -p`
- Claude hooks in `.claude/settings.json`

See [CODEX_WORKFLOWS.md](CODEX_WORKFLOWS.md) for the main Codex-native search and application-fill workflows.

## How to Use This Repo in Codex

1. Open the repo in Codex.
2. Let Codex read `AGENTS.md`, `modes/_shared.md`, and the relevant mode file.
3. Ask for actions in plain language instead of slash commands.

Examples:
- "Evaluate this job URL and generate the tailored PDF"
- "Scan my configured portals and add promising roles to the pipeline"
- "Process the pending URLs in `data/pipeline.md`"
- "Update my archetypes for backend platform roles"
- "Help me fill this application form and stop before submit"

## Feature Mapping

Claude UX:
- `/career-ops <jd>` -> Codex: "Evaluate this JD and run the full pipeline"
- `/career-ops scan` -> Codex: "Scan portals for new offers"
- `/career-ops pdf` -> Codex: "Generate the PDF for this role"
- `/career-ops tracker` -> Codex: "Show tracker status"
- `/career-ops batch` -> Codex: "Process these offers in batch"

Codex caveat:
- Batch can be done manually or with Codex sub-agents, but the shipped `batch-runner.sh` is not Codex-native.
- Search and application-assist work best as interactive Codex sessions rather than one-shot CLI commands.

## Recommended Fork Changes

If you fork this repo to make it Codex-first, do these first:

1. Keep `CLAUDE.md`, but add `AGENTS.md` as the primary instruction file.
2. Keep the existing mode files; they are reusable with minor wording changes.
3. Replace slash-command wording in docs with plain-language invocation examples.
4. Either rewrite `batch/batch-runner.sh` for your chosen runtime or mark batch as manual-only.
5. Update `update-system.mjs` to track your fork's canonical repo and include Codex-specific files.

## Honest Portability Summary

- Core system: portable
- Agent prompts and repo habits: portable with light edits
- CLI automation and slash-command layer: not portable without explicit replacement
