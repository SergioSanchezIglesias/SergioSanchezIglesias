# GitHub Profile Refresh

## Objective
Refresh the GitHub profile README to present Sergio as a Software Engineer working at the intersection of front-end engineering and AI-assisted software delivery.

## Problem and rationale
The current profile is badge-heavy, bilingual, and does not foreground projects, technical focus, or contribution activity. The requested reference favors a concise editorial structure with readable technology context and visual evidence.

## Scope
- Rewrite `README.md` in English with a clear professional headline, project section, Tech DNA, and activity cards.
- Generate public activity summary cards daily or manually with GitHub Actions, publishing only the `github_dark` theme to the `output` branch.
- Replace the unavailable hosted statistics card with two raw SVG summary cards, retain the streak card, and remove the former contribution animation.
- Leave GitHub-native pinned repositories to profile configuration rather than duplicating them in the README.

## Constraints
- Preserve the existing LinkedIn URL.
- Do not touch the unrelated untracked `.gitignore`.
- Do not claim private contribution counts: external cards display public data unless separately self-hosted and token-configured.
- Technical artifacts remain English.

## Tasks
- [x] P1 — Rewrite the profile README with the new information hierarchy and Software Engineer + AI positioning.
- [x] P2 — Superseded by P4: the original contribution graph workflow and embedded asset are no longer part of the intended profile.
- [x] P3 — Verify Markdown links, image URLs, workflow syntax, and the final diff.
- [x] P4 — Replace the unavailable hosted stats card with GitHub Actions-generated profile summary cards and remove the former contribution animation.
- [x] P5 — Verify the revised README and workflow configuration locally; remote execution and rendering remain untested.

## Acceptance criteria and checks
- The README starts with a concise Software Engineer positioning that explicitly includes front-end engineering and AI.
- It includes What I Build, Tech DNA, two public activity summary cards, the existing streak card, and clear pinned-repository guidance; the former animation and unavailable hosted stats card are absent.
- README embeds accessible raw SVGs from `output/profile-summary-card-output/github_dark/`: `3-stats.svg` and `2-most-commit-language.svg`, with concise daily-refresh/public-data context.
- The workflow uses `actions/checkout@v4` and `vn7n24fzkq/github-profile-summary-cards@release`, runs daily and manually, and publishes the `github_dark` theme to `output`.
- Generation uses `${{ github.repository_owner }}` and the automatic `GITHUB_TOKEN`, requiring no user-created token or secret. Write permission is job-scoped; top-level contents permission is read-only.
- Verification includes a Markdown/link and YAML parse check where tooling is available, plus a diff review.

## Progress
- 2026-09-15 — User selected the `Software Engineer` positioning. Work initialized; no implementation completed at initialization.
- P1/P2 completed: editorial README and contribution workflow implemented within the authorized scope.
- P3 completed: local source/link inspection, YAML parsing, whitespace validation, tracked diff inspection, and independent verification passed. Remote rendering and workflow execution remain untested.

## Verification evidence
- Strict TDD disabled by explicit user choice for this README/workflow documentation task.
- P1: README now includes English positioning, About, What I Build, Tech DNA, public stats/streak cards, pinned-repository guidance, and the original LinkedIn URL.
- P2 (historical, superseded): the original daily/manual contribution-graph workflow and README asset were replaced by P4.
- Original source inspection confirmed the LinkedIn URL and descriptive image alt text; the former generated-asset checks are superseded by P5.
- Live URL availability and GitHub Actions execution have not been tested. The output asset requires a successful workflow run.
- `ruby -e "require 'yaml'; YAML.load_file('.github/workflows/contribution-snake.yml'); puts 'YAML OK'"`: exited 0, printed `YAML OK`.
- `git diff --check`: initially reported trailing whitespace in README line 3; removed it and reran successfully (exit 0, no output).
- `git diff -- README.md .github/workflows/contribution-snake.yml odd/tasks/github-profile-refresh.md`: exited 0; inspected the README diff. Git omits untracked files, so the new workflow and this task document were inspected directly instead.
- `.gitignore` remained untouched; no files staged or committed.
- Independent verification passed: YAML parsing and `git diff --check` succeeded; README, untracked workflow, and task document were inspected directly. The verifier found no blocker. It confirmed the positioning is not inflated, the LinkedIn URL is preserved, URLs/branch match, and write permission is job-scoped. The untracked `.gitignore` has no baseline, so its unchanged status cannot be independently proven.
- Native review assessment was unavailable because the package-local Gentle AI binary is missing; risk was treated as unassessable and an independent verifier was run.
- User reported broken activity images. Prior investigation found the hosted stats service returned HTTP 503 (`DEPLOYMENT_PAUSED`) and the former generated asset returned 404. User authorized Action-generated summary cards instead.
- P4 completed: README now embeds two raw summary-card SVGs with descriptive alt text, preserves the streak card and LinkedIn URL, and explains daily refreshes and public data. The existing workflow filename is retained, but its content now generates summary cards using the automatic token, output branch, and only the github_dark theme.
- P5 completed locally: the exact Ruby YAML parse command above exited 0 and printed `YAML OK`; `git diff --check` exited 0 with no output; the scoped `git diff` command above exited 0 and showed all three tracked files. Source inspection confirmed the requested URLs, accessible alt text, preserved LinkedIn/streak links, daily/manual triggers, output branch, theme, automatic token, and job-scoped write permission.
- Independent verification of P4/P5 passed: YAML parsing and `git diff --check` succeeded, the three scoped files were inspected, and the verifier found no blocker. It confirmed removal of the former hosted/snake references, retained LinkedIn/streak links, exact card output paths, descriptive alt text, requested actions, restricted theme, automatic token, and top-level read/job-scoped write permissions.
- YAML parsing does not validate the external action's runtime behavior. Live rendering and remote workflow execution remain untested; summary-card assets require the first successful workflow run.

## Next step
Manually dispatch the workflow on GitHub to publish the initial cards and confirm remote rendering. No files were staged or committed, and `.gitignore` was not modified.
