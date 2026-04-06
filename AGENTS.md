# Career-Ops for Codex

This repo was authored for Claude Code, but the core workflow is portable to Codex.

Use this file as the primary repo instruction entrypoint in Codex. Keep `CLAUDE.md` for upstream compatibility.

## What Works in Codex

- Offer evaluation from pasted JD text or URLs
- Report generation in `reports/`
- PDF generation via `node generate-pdf.mjs`
- Tracker integrity scripts (`merge`, `verify`, `normalize`, `dedup`)
- Dashboard TUI in `dashboard/`
- Repo customization by editing `config/profile.yml`, `modes/_profile.md`, `portals.yml`, and templates

## What Is Not Codex-Native Yet

- The `/career-ops ...` slash-command UX from `.claude/skills/career-ops/SKILL.md`
- The standalone batch runner in `batch/batch-runner.sh`, which shells out to `claude -p`
- Claude-specific hooks in `.claude/settings.json`

In Codex, use natural-language requests instead of slash commands. Examples:

- "Evaluate this JD and generate the report and PDF"
- "Scan portals for new offers"
- "Process pending URLs from `data/pipeline.md`"
- "Generate a tailored PDF for this role"
- "Show me tracker status"

## Data Contract

Read `DATA_CONTRACT.md` before making structural changes.

Personalization belongs in the user layer:
- `cv.md`
- `config/profile.yml`
- `modes/_profile.md`
- `article-digest.md`
- `portals.yml`

System behavior belongs in the system layer:
- `modes/_shared.md` and mode files
- `AGENTS.md`
- `CLAUDE.md`
- scripts, templates, batch files, dashboard, docs

Do not put user-specific narrative or preferences into `modes/_shared.md`.

## Onboarding Checks

Before running an evaluation, check that these exist:

1. `cv.md`
2. `config/profile.yml`
3. `modes/_profile.md`
4. `portals.yml`

If `modes/_profile.md` is missing, copy `modes/_profile.template.md`.

If any required file is missing, guide setup before running the pipeline.

## Mode Routing

Use the same mode concepts as the Claude version, but route from user intent instead of `/career-ops` commands:

- Pasted JD or JD URL: `auto-pipeline`
- "evaluate this offer": `oferta`
- "compare these offers": `ofertas`
- "generate/update the PDF": `pdf`
- "scan portals": `scan`
- "process pipeline": `pipeline`
- "fill application form": `apply`
- "company research": `deep`
- "tracker": `tracker`
- "batch": manual multi-offer orchestration in Codex, not `batch-runner.sh`

Read `modes/_shared.md` plus the relevant mode file before executing a mode.

## Global Rules

- Never invent experience, metrics, or credentials.
- Never submit an application without explicit user confirmation.
- For the first evaluation in a session, run `node cv-sync-check.mjs`.
- After evaluations, write tracker additions via TSV in `batch/tracker-additions/` and merge them; do not manually add new rows directly to `data/applications.md`.
- Use the JD language for generated outputs. Default to English.
- Be direct and actionable.

## Batch Guidance in Codex

Treat `batch/batch-runner.sh` as Claude-only unless it is rewritten.

If the user wants batch processing in Codex:

1. Read `batch/batch-input.tsv`
2. Process offers sequentially or with Codex sub-agents
3. Generate the same outputs expected by the repo:
   - report `.md`
   - PDF in `output/`
   - tracker TSV in `batch/tracker-additions/`
4. Run `node merge-tracker.mjs`
5. Run `node verify-pipeline.mjs`

## Migration Note

For a real Codex-first fork, keep both `AGENTS.md` and `CLAUDE.md` at first. Remove Claude-specific workflow pieces only after replacing them with Codex-native equivalents.
