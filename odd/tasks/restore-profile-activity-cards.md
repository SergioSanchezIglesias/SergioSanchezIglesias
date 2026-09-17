# Restore profile activity cards

## Objective
Restore the two broken GitHub Activity summary cards in the profile README and remove the closing slogan requested by the user.

## Problem and rationale
The README references SVG assets on the `output` branch that return `404: Not Found`; GitHub consequently displays broken images. The card-generation workflow is absent. The closing slogan is unwanted.

## Scope
- Add a GitHub Actions workflow that regenerates and publishes profile-summary-card SVGs to `output` daily and on manual dispatch.
- Remove the unwanted closing slogan from `README.md`.

## Constraints
- Preserve the existing dark-theme card URLs and the working external streak card.
- Do not commit, push, or create a pull request unless explicitly requested.

## Tasks
- [x] T1 Add automated profile-summary-card publishing workflow and keep README card references valid.
- [ ] T2 Remove the closing slogan and verify the rendered asset endpoints.

## Acceptance criteria and checks
- Both `github_dark/3-stats.svg` and `github_dark/2-most-commit-language.svg` resolve after the workflow publishes its first run.
- The README contains no `Build with intent. Keep learning. Leave the code clearer than you found it.` text.
- Workflow YAML is syntactically valid by inspection; GitHub Actions execution is pending remote dispatch.

## Delivery
- Forecast: approximately 45 authored changed lines.
- Strategy: ask-on-risk.
- TDD: disabled by user decision; validate through focused YAML/README inspection and a text search because this profile repository has no application test runner.

## Progress and evidence
- 2026-09-17: Confirmed both current raw SVG URLs return `404: Not Found`. The streak card endpoint responds.
- 2026-09-17: User selected disabled TDD; use focused YAML/README inspection and text search.

- Added `.github/workflows/profile-summary-cards.yml`: daily at 06:00 UTC and manual dispatch, `contents: write`, `GITHUB_TOKEN`, explicit `output` branch, and automatic publishing through `vn7n24fzkq/github-profile-summary-cards@release`.
- Focused inspection of the workflow YAML and README passed: triggers, publishing permissions/token, repository-owner username, and existing `profile-summary-card-output/github_dark` SVG paths are coherent. The write tool also reported YAML clean.
- Local literal search in `README.md` for the exact closing slogan returned no matches. Removed the now-unused closing divider; preserved the two summary-card URLs and external streak card.
- T2 is partially complete: slogan removal is verified locally; published endpoint verification remains pending a successful remote workflow run. No remote execution or endpoint success is claimed.
- RED: not active — strict TDD was not activated. GREEN: not active — validation is reported separately.
- Preserved the pre-existing untracked `.gitignore`. No commit, push, workflow dispatch, or pull request was performed.
- Independent local verification passed: confirmed action, triggers, permissions, token, output branch, unchanged streak URL, expected dark-card paths, absent slogan, and no unsubstantiated remote-success claim.
- Native review assessment was unavailable because the local Gentle AI binary is missing; therefore the independent verifier was run and passed.

## Next step
After the workflow is delivered to the default branch with user authorization, run it remotely (manual dispatch or scheduled run). Confirm that both README SVG endpoints resolve, then mark T2 complete. Local inspection cannot prove published cards resolve.
