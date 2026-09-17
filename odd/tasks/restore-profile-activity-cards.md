# Restore profile activity cards

## Objective
Restore the two broken GitHub Activity summary cards in the profile README and remove the closing slogan requested by the user.

## Problem and rationale
The README references SVG assets on the `output` branch that return `404: Not Found`; GitHub consequently displays broken images. The card-generation workflow is absent. The closing slogan is unwanted.

## Scope
- Add a GitHub Actions workflow that regenerates and publishes profile-summary-card SVGs to `profile-summary-card-output` daily and on manual dispatch.
- Remove the unwanted closing slogan from `README.md`.

## Constraints
- Preserve the dark theme and working external streak card; correct summary-card URLs to the published branch/root layout.
- Do not commit, push, or create a pull request unless explicitly requested.

## Tasks
- [ ] T1 Correct the profile-summary-card workflow and README asset paths after the first remote run failed.
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
- Remote workflow run `35233548227` failed during `git add ./profile-summary-card-output/` (exit 128). The action's official workflow requires an `actions/checkout` step, token input `TOKEN`, and its recommended `profile-summary-card-output` branch/path convention. Reopen T1 to correct this configuration and README paths.
- Initial inspection (superseded by the remote failure and correction below) of the workflow YAML and README passed: triggers, publishing permissions/token, repository-owner username, and existing `profile-summary-card-output/github_dark` SVG paths are coherent. The write tool also reported YAML clean.
- Local literal search in `README.md` for the exact closing slogan returned no matches. Removed the now-unused closing divider; preserved the two summary-card URLs and external streak card.
- T2 is partially complete: slogan removal is verified locally; published endpoint verification remains pending a successful remote workflow run. No remote execution or endpoint success is claimed.
- RED: not active — strict TDD was not activated. GREEN: not active — validation is reported separately.
- Preserved the pre-existing untracked `.gitignore`. No commit, push, workflow dispatch, or pull request was performed.
- Independent local verification passed: confirmed action, triggers, permissions, token, output branch, unchanged streak URL, expected dark-card paths, absent slogan, and no unsubstantiated remote-success claim.
- Native review assessment was unavailable because the local Gentle AI binary is missing; therefore the independent verifier was run and passed.

## Correction after remote failure
- Added `actions/checkout@v4` before generation and replaced the env-only token with the documented `TOKEN: ${{ secrets.GITHUB_TOKEN }}` action input.
- Set `BRANCH_NAME: "profile-summary-card-output"`, retained `AUTO_PUSH: true`, and corrected both README raw URLs to `profile-summary-card-output/github_dark/<card>.svg` (assets at the branch root).
- Preserved the external streak card and absence of the closing slogan. Local YAML/README inspection passed; the editor reported YAML clean. These checks do not establish remote execution success.
- T1 configuration correction is complete locally; T1 and T2 remain open pending successful remote publication and endpoint verification.

## Next step
After the correction is delivered remotely, rerun the workflow. Confirm that both README SVG endpoints resolve before marking T1 and T2 complete. Remote rerun remains necessary; no commit, push, dispatch, or PR was performed for this correction.
