# Codex Setup

Career-Ops is Codex-ready through the root `AGENTS.md` file. Codex should use
that file as the canonical instruction source, then route into the same
checked-in `modes/*.md`, templates, tracker flow, and scripts used by other
AI coding CLIs.

Do not build a separate Codex automation layer. The durable contract is:

1. `AGENTS.md` defines behavior and data safety.
2. `modes/*.md` define task-specific workflows.
3. Node scripts perform deterministic work.
4. User personalization stays in user-layer files.

## Prerequisites

- Codex CLI or a Codex client that reads project `AGENTS.md`
- Node.js 18+
- Playwright Chromium installed for PDF generation and reliable job verification
- Go 1.21+ if you want the TUI dashboard

## Install

```bash
npm install
npx playwright install chromium
```

## Start Codex

```bash
cd career-ops
codex
```

For non-interactive execution, use the headless command listed in `AGENTS.md`:

```bash
codex exec "Run Career-Ops scan and summarize new pipeline entries."
```

## Recommended Starting Prompts

- `Evaluate this job URL with Career-Ops and run the full pipeline.`
- `Scan my configured portals for new roles that match my profile.`
- `Generate the tailored ATS PDF for this role using Career-Ops.`
- `Show my tracker status and recommend next actions.`

## Routing Map

| User intent | Files Codex should read |
|-------------|-------------------------|
| Raw JD text or job URL | `modes/_shared.md` + `modes/auto-pipeline.md` |
| Single evaluation only | `modes/_shared.md` + `modes/oferta.md` |
| Multiple offers | `modes/_shared.md` + `modes/ofertas.md` |
| Portal scan | `modes/_shared.md` + `modes/scan.md` |
| PDF generation | `modes/_shared.md` + `modes/pdf.md` |
| Live application help | `modes/_shared.md` + `modes/apply.md` |
| Pipeline inbox processing | `modes/_shared.md` + `modes/pipeline.md` |
| Tracker status | `modes/tracker.md` |
| Deep company research | `modes/deep.md` |
| Training / certification review | `modes/training.md` |
| Project evaluation | `modes/project.md` |

The key point: Codex support is additive. It should route into the existing
Career-Ops modes and scripts rather than introducing a parallel automation
layer.

## Codex Operating Notes

- Use natural-language prompts; there is no required Codex slash-command layer.
- Read `AGENTS.md` first, then the relevant mode file.
- Prefer deterministic scripts for scanner, PDF, tracker merge, normalization, and verification.
- Use Playwright for live job-post verification when available.
- Keep generated reports in `reports/`, PDFs in `output/`, and tracker additions in `batch/tracker-additions/`.
- Respect `.gitignore`; never commit CVs, profiles, reports, generated PDFs, or interview prep.

## Behavioral Rules

- Treat raw JD text or a job URL as the full auto-pipeline path unless the user explicitly asks for evaluation only.
- Keep all personalization in `config/profile.yml`, `modes/_profile.md`, `article-digest.md`, or `portals.yml`.
- Never verify a job’s live status with generic web fetch when Playwright is available.
- Never submit an application for the user.
- Never add new tracker rows directly to `data/applications.md`; use the TSV addition flow and `merge-tracker.mjs`.

## Verification

```bash
npm run verify

# optional dashboard build
cd dashboard && go build ./...
```
