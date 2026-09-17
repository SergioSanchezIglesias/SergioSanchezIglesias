# GitHub Profile Refresh

## Objective
Refresh the GitHub profile README to present Sergio as a Software Engineer working at the intersection of front-end engineering and AI-assisted software delivery.

## Problem and rationale
The current profile is badge-heavy, bilingual, and does not foreground projects, technical focus, or contribution activity. The requested reference favors a concise editorial structure with readable technology context and visual evidence.

## Scope
- Rewrite `README.md` in English with a clear professional headline, project section, Tech DNA, and activity cards.
- Add a GitHub Actions workflow that generates a contribution-history visualisation from the public GitHub contribution graph.
- Leave GitHub-native pinned repositories to profile configuration rather than duplicating them in the README.

## Constraints
- Preserve the existing LinkedIn URL.
- Do not touch the unrelated untracked `.gitignore`.
- Do not claim private contribution counts: external cards display public data unless separately self-hosted and token-configured.
- Technical artifacts remain English.

## Tasks
- [x] P1 — Rewrite the profile README with the new information hierarchy and Software Engineer + AI positioning.
- [x] P2 — Add an automated contribution graph visualisation workflow and embed its generated asset.
- [x] P3 — Verify Markdown links, image URLs, workflow syntax, and the final diff.

## Acceptance criteria and checks
- The README starts with a concise Software Engineer positioning that explicitly includes front-end engineering and AI.
- It includes What I Build, Tech DNA, contribution statistics/streak cards, a contribution-history visualisation, and clear pinned-repository guidance.
- The workflow runs on schedule and manual dispatch, writes only its generated SVG under `dist/`, and has least-privilege `contents: write` permission.
- Verification includes a Markdown/link and YAML parse check where tooling is available, plus a diff review.

## Progress
- 2026-09-15 — User selected the `Software Engineer` positioning. Work initialized; no implementation completed at initialization.
- P1/P2 completed: editorial README and contribution workflow implemented within the authorized scope.
- P3 completed: local source/link inspection, YAML parsing, whitespace validation, tracked diff inspection, and independent verification passed. Remote rendering and workflow execution remain untested.

## Verification evidence
- Strict TDD disabled by explicit user choice for this README/workflow documentation task.
- P1: README now includes English positioning, About, What I Build, Tech DNA, public stats/streak cards, pinned-repository guidance, and the original LinkedIn URL.
- P2: Workflow created with daily/manual triggers, repository-owner input, job-scoped contents write permission, SVG generation under `dist/`, and publication to `output`. README embeds the exact dark SVG URL.
- Manual source inspection confirmed the LinkedIn URL is preserved, both activity cards use `SergioSanchezIglesias`, and the snake URL matches the workflow's branch and filename. All images have descriptive alt text.
- Live URL availability and GitHub Actions execution have not been tested. The output asset requires a successful workflow run.
- `ruby -e "require 'yaml'; YAML.load_file('.github/workflows/contribution-snake.yml'); puts 'YAML OK'"`: exited 0, printed `YAML OK`.
- `git diff --check`: initially reported trailing whitespace in README line 3; removed it and reran successfully (exit 0, no output).
- `git diff -- README.md .github/workflows/contribution-snake.yml odd/tasks/github-profile-refresh.md`: exited 0; inspected the README diff. Git omits untracked files, so the new workflow and this task document were inspected directly instead.
- `.gitignore` remained untouched; no files staged or committed.
- Independent verification passed: YAML parsing and `git diff --check` succeeded; README, untracked workflow, and task document were inspected directly. The verifier found no blocker. It confirmed the positioning is not inflated, the LinkedIn URL is preserved, URLs/branch match, and write permission is job-scoped. The untracked `.gitignore` has no baseline, so its unchanged status cannot be independently proven.
- Native review assessment was unavailable because the package-local Gentle AI binary is missing; risk was treated as unassessable and an independent verifier was run.

## Next step
After publication, run the workflow manually to create the output assets and check live profile rendering.
