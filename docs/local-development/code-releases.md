# Code Releases

## Important notes

- Make sure releases/merges to `staging` and `production` are of the "merge commit" type.
  - The green button for merging the PR will be labeled "Merge pull request" and in the drop down it is named "Create a merge commit".
- Feature merges into `develop` should be merged using the rebase method where the green button will be labeled "Rebase and merge".
  - This is done so that `develop` has all our features commits and the commits in `production` & `staging` stay as clean as possible.
- The [GitHub CLI](https://cli.github.com/) is a great tool to remove redundancies and manual parts of this procedure. This guide will give examples for with/without that tool.

## Defining the Changelog

- There should be a release template for the merge description under `~/.github/PULL_REQUEST_TEMPLATE/release_template.md`
- Gather the Pull Requests(PRs) that were merged into `develop` since the last release (i.e. those that have the HEAD as `develop`), using your preferred method. Options, but not limited to:
  - Using `GitHub CLI`.
    - `gh pr list --state merged --search "NOT staging in:title sort:updated-desc"` (at time of publishing) will list most of the information needed.
  - Manually through `GitHub UI`.
    - Visit the repo URL `{repo-url}/pulls?q=is%3Apr+is%3Amerged+NOT+staging+in%3Atitle+sort%3Aupdated-desc+`
    - Collect the #, title, and note what it affected (theme, plugin, workflow, etc) to match with the Release PR template.
- Make sure to exclude any PRs that were merged into `production` since they should not be in the release notes/changelog.
- Place the PR messages into the corresponding section of the Release Template.
- If applicable, add mentions or closing keywords for issues in the corresponding `## Issues` section.
- `GitHub CLI use`: Save as markdown file and note its path.

---

## Release steps

### GitHub CLI

Run the following command line commands to generate the PRs and create the release. Commands should work in both Mac and Windows. Replace the whole string(s) in the commands with the appropriate values, yes including the curly brackets: `{note}`.

The `--draft` flag is optional but highly recommended. Each command will echo the URL of the actual PR.

- `gh pr create --draft --title "{release version/number} pre-release" --body-file ~/{path/to/release/notes.md} --assignee @me --base staging --head develop`
- `gh pr create --draft --title "{release version/number}" --body-file ~/{path/to/release/notes.md} --assignee @me --base production --head develop`

Example:

- `gh pr create --draft --title "v1.10.0 - pre-release" --body-file ~/Desktop/kanahoma.org-v1.10.0.md --assignee @me --base staging --head develop`

Once the last PR is merged into `production`, run the command below to create a release.

- `gh release create {release version/number} --title "{release version/number} - {name of release}" --draft --notes-file ~/{path/to/release/notes.md}`

Example:

- `gh release create v1.10.0 --title "v1.10.0" --draft --notes-file ~/Desktop/nu.edu-v1.10.0.md`

### Manual

- First do a `vX.XX.XX - pre-release` PR from `develop` to `staging` so the environments stay in sync and let any CI tests complete.
  - Paste the previously determined release notes into the description.
  - Assign yourself to the PR.
- After the above, do a PR from `develop` to `production` with the release number `vX.XX.XX`.
  - Paste the previously determined release notes into the description.
  - Again, assign yourself to the PR.
- Tag the release under `/releases`
  - Use the same release notes as the PR into `production` branch.

### After GitHub Release Creation

- If applicable, monitor the CI dashboard for completion.
- Run any post-launch tests.
  - Confirm that features in the release are working either manually or through any CI processes.
  - If Cypress testing workflow is set up for that website, run the reports generator script for the corresponding site.
- If accessible, add an annotation of the release and version number to Google Analytics.
  - *As of the beginning of December 2023, this capability is not available in GA4. It is strongly advised to set up an event in your Outlook calendar instead.*

## Housekeeping

- Clean unused and already merged `branches`. Make sure you exercise good housekeeping by deleting unused branches locally and remotely.
- Close `issues`. Make sure you close those issues that have been fixed, in case they were not automatically closed via keywords in the release notes.

---

> Last Modified: {docsify-updated}
