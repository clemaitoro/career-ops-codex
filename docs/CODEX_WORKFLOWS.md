# Codex Workflows

This file explains the two workflows that matter most in Codex: job search and application assist.

## 1. Search Jobs

Use this when you want Career-Ops to find new roles and add them to the pipeline.

### What Codex should do

1. Read `portals.yml`
2. Read `data/scan-history.tsv` if present
3. Read `data/pipeline.md` and `data/applications.md` if present
4. Use the discovery strategy in `modes/scan.md`
5. Filter by your configured title rules
6. Deduplicate against:
   - scan history
   - pipeline
   - existing applications
7. Add only new relevant jobs to `data/pipeline.md`
8. Record scan outcomes in `data/scan-history.tsv`

### Good prompts

- `Scan my configured portals for backend platform and AI infrastructure roles, then add strong matches to data/pipeline.md.`
- `Search tracked companies for new roles, skip duplicates, and summarize the additions.`
- `Run a job scan using portals.yml and tell me which jobs are worth evaluating next.`

### Expected output

- New jobs added to `data/pipeline.md`
- Updated `data/scan-history.tsv`
- A short summary of:
  - total found
  - relevant
  - duplicates
  - newly added

## 2. Fill Applications

Use this when you already have a job and want browser-assisted form help.

### What Codex should do

1. Open or inspect the application form
2. Identify company and role
3. Find the matching report in `reports/`
4. Read:
   - `cv.md`
   - `config/profile.yml`
   - the report
5. Read visible questions on the page
6. Draft tailored answers
7. If asked, fill visible fields in the browser
8. Stop before final submit

### Good prompts

- `Help me fill this application and stop before submit.`
- `Read this form, match it to the existing report, and draft answers for each question.`
- `Fill the visible fields from my profile and CV, then wait for my approval before the final step.`

### What “autofill” means here

It means:
- prefill standard identity details
- draft or fill text answers
- help upload the right CV/PDF
- keep the user in control

It does not mean:
- silently applying to jobs in the background
- auto-submitting without review
- mass-applying to many roles at once

## Recommended Day-to-Day Flow

1. `Scan my portals and add strong matches to data/pipeline.md.`
2. `Process the top 3 pending jobs in data/pipeline.md and generate reports and PDFs.`
3. `Summarize the strongest jobs to apply to next.`
4. `Help me fill this application and stop before submit.`

## Current Limitation

In Codex, this workflow is interactive rather than slash-command driven. The old Claude command layer is still present for compatibility, but the practical Codex interface is plain English requests.
