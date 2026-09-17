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
- [x] T1 Correct the profile-summary-card workflow and README asset paths after the first remote run failed.
- [x] T2 Remove the closing slogan and verify the rendered asset endpoints.
- [x] T3 Remove the activity explanatory note and arrange the two summary cards side by side above the streak card.
- [x] T4 Retain the least-privilege `PROFILE_STATS_TOKEN` configuration and accept partial private-activity metrics after capability investigation.

## Acceptance criteria and checks
- Both `github_dark/3-stats.svg` and `github_dark/2-most-commit-language.svg` resolve after the workflow publishes its first run.
- The README contains no `Build with intent. Keep learning. Leave the code clearer than you found it.` text.
- Workflow YAML is syntactically valid by prior inspection; remote run `35234481642` successfully published the cards.

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
- Remote rerun `35234091070` completed checkout but failed the action step with unexpected input `TOKEN` and `GITHUB_TOKEN is missing`. This supersedes the earlier token-input guidance: the installed `@release` action requires `GITHUB_TOKEN` as an environment variable.
- Removed unsupported `with.TOKEN` and restored action-step `env.GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}`. Preserved `actions/checkout@v4`, the action's `@release` reference, repository-owner username, `profile-summary-card-output` branch, and `AUTO_PUSH: true`.
- Focused YAML inspection confirmed the unsupported token input is absent and the environment token is present; the editor reported YAML clean. Remote rerun remains required; no remote success is claimed.
- Preserved the external streak card and absence of the closing slogan. Local YAML/README inspection passed; the editor reported YAML clean. These checks do not establish remote execution success.
- T1 configuration correction is complete locally; T1 and T2 remain open pending successful remote publication and endpoint verification.

## Verified remote publication and final URL correction
- Parent-provided remote evidence supersedes the pending-run statements above: workflow run `35234481642` succeeded and generated the cards in `origin/main` commit `974924d`, under `profile-summary-card-output/github_dark/`.
- The assumed separate `profile-summary-card-output` branch does not exist; README URLs using that branch remained HTTP 404. Updated only the two README summary-card URLs to the observed `main/profile-summary-card-output/github_dark/3-stats.svg` and `main/profile-summary-card-output/github_dark/2-most-commit-language.svg` locations.
- Both corrected raw URLs already returned HTTP 200 in the supplied remote checks. These are previously observed results, not new HTTP checks or a new workflow run performed during this correction.
- Local README inspection confirms the exact corrected URLs, unchanged external streak card, and absent closing slogan. Together with prior YAML inspection and the supplied successful remote publication/endpoint evidence, all acceptance criteria are observed; T1 and T2 are complete.
- No workflow, generated assets, or pre-existing untracked `.gitignore` changes were made during this final correction. No commit, push, dispatch, or pull request was performed.

## Compact activity layout verification
- Replaced the summary-card Markdown images with one centered HTML paragraph containing both images at their natural 340px width; placed the existing streak image in its own centered paragraph directly below.
- Removed the explanatory italic activity note completely.
- Read the changed README in full: PASS — one summary-card row, one streak-card row, no explanatory note, all three image URLs and alt texts unchanged, and the heading and all other README content preserved. This is local markup inspection, not a rendered-browser or remote endpoint check.
- TDD remains disabled. RED: not active — strict TDD was not activated. GREEN: not active — validation is reported separately.
- Preserved existing task-document progress and the pre-existing untracked `.gitignore`. No other files were edited; no commit, push, workflow dispatch, or pull request was performed.

## Private activity token wiring verification
- Changed only the generation action's `env.GITHUB_TOKEN` reference from `${{ secrets.GITHUB_TOKEN }}` to `${{ secrets.PROFILE_STATS_TOKEN }}`. Preserved checkout, action reference, username, branch, auto-push, and every other workflow setting.
- Full local YAML inspection: PASS — the generation step references the named repository secret, with no literal secret value present. The editor reported YAML clean. The secret value was never accessed, printed, or copied.
- The user created the secret after being advised that aggregate private-work statistics may become public. Only a remote run can validate private activity inclusion; no such validation is claimed here.
- TDD remains disabled. RED: not active — strict TDD was not activated. GREEN: not active — validation is reported separately.
- Preserved prior task-record changes and the pre-existing untracked `.gitignore`. No secrets were modified and no workflow dispatch, commit, push, or pull request was performed.

## Private activity validation
- User selected all-repository access and reran workflow `35237116496`; the run succeeded, but the generated Stats values remained `0` stars, `27` commits, `4` PRs, `3` issues, and `4` contributed repositories. No private-activity inclusion can be confirmed.
- Read-only investigation found 14 private repositories visible through the local GitHub account: 7 under `SergioSanchezIglesias` and 7 under other owners (`JulianRoman26`, `archyxsec`, and `msiglesias`). A fine-grained PAT has one resource owner, so it cannot grant one workflow access across all of those owners without each organization's approval/authorization.
- The action source permanently restricts Total Stars to public non-fork owned repositories and Contributed to to public repositories. Those two values cannot include private repositories even with a broader token. Commit, PR, issue, and language data depend on what the token can view through GitHub's GraphQL API.
- T4 remains open pending a user decision: retain least-privilege partial metrics, or replace the fine-grained PAT with a broader classic `repo` PAT and authorize it for each organization where policy permits.

## Next step
No further token or workflow changes are planned. The current card retains the fine-grained token and partial metrics by explicit user decision.
