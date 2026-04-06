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
- Browser-assisted application filling with user review before submit

## What Is Not Codex-Native Yet

- The `/career-ops ...` slash-command UX from `.claude/skills/career-ops/SKILL.md`
- The standalone batch runner in `batch/batch-runner.sh`, which shells out to `claude -p`
- Claude-specific hooks in `.claude/settings.json`
- One-command slash-command routing for search/apply; in Codex this is plain-language intent routing

In Codex, use natural-language requests instead of slash commands. Examples:

- "Evaluate this JD and generate the report and PDF"
- "Scan portals for new offers"
- "Process pending URLs from `data/pipeline.md`"
- "Generate a tailored PDF for this role"
- "Show me tracker status"
- "Scan my portals config and add relevant jobs to `data/pipeline.md`"
- "Open this application form, read the questions, and draft or fill answers, but stop before submit"

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

## Primary Codex Workflows

### Job Search

When the user wants job discovery:

1. Read `portals.yml`
2. Read `data/scan-history.tsv` if it exists
3. Read `data/pipeline.md` and `data/applications.md` if they exist
4. Execute the discovery strategy from `modes/scan.md`
5. Add only relevant, deduped jobs to `data/pipeline.md`
6. Record outcomes in `data/scan-history.tsv`
7. Summarize what was added and what was skipped

Recommended user phrasing:
- "Scan my portals for new backend platform roles"
- "Search configured companies and add strong matches to the pipeline"

### Application Assist / Autofill

When the user wants help filling an application:

1. Read `modes/apply.md`
2. Prefer a visible browser session so the user can see every action
3. Identify company and role from the form page
4. Load the matching report from `reports/` if one exists
5. Read `cv.md` and `config/profile.yml`
6. Draft answers for visible questions
7. If the user wants it, fill fields in the browser
8. Stop before final submit unless the user gives an explicit final confirmation

Important distinction:
- "autofill" means browser-assisted filling and answer drafting
- it does not mean background mass-application or silent submission

Recommended user phrasing:
- "Help me fill this form and stop before submit"
- "Open this application page, read the questions, and draft answers from my report"
- "Fill the visible fields from my profile and CV, then wait for my review"

## Global Rules

- Never invent experience, metrics, or credentials.
- Never submit an application without explicit user confirmation.
- Never run blind mass-apply behavior across multiple jobs.
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
