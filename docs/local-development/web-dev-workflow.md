# Web Development Workflow

Our websites code on the VIP Platform can only be modified using a version-controlled GitHub development workflow. **There is no SFTP access to an application’s code or other related assets.**

## Dev Enviroments

In addition to `production`, we have two development environments: `develop` and `staging`

Each environment is its own standalone site, with its own codebase and its own database.

- Each environment has it's own branch in our repo
  - Technically two branches: `develop` & `develop-built` or `staging` & `staging-built`
  - This is because of our build process
  - `staging`, `develop` and `production` are what we create pull requests towards
  - `staging-built`, `develop-built`, and `production-built` are what are actually get deployed and shown to the final user

## Workflow for Web Development

This workflow ensures a controlled and organized development process, with stages for development, testing, and deployment.

1. **Clone the Repository:**  
   Each developer clones the main repository to their local machine.

2. **Create Feature Branches:**  
   Developers create individual feature branches for each task or feature they are working on.  
   Create a new branch based off `develop` branch  
   Branch names should be descriptive and follow our [naming convention](local-development/web-dev-workflow?id=branch-naming-convention).

3. **Development and Commits:**  
   Developers work on their features locally within their feature branches.  
   Regularly commit and push changes to the remote repository. **Commit early, commit often.**

4. **Create Pull Request (PR):**  
   Once a feature is complete and tested locally, the developer creates a pull request from their feature branch to the `develop` branch.
   - Pull request should include a description of what the merge will affect (use PR templates).
   - Assign the project or dev lead as a reviewer.
   - Code review to ensure "best practice" standards, and no clashes with other code. Await reviewers' approval for merge.
   - Add labels if applicable (at least a priority label).
     - **Priority 1** - High priority - Should be released ASAP (e.g. bug fixes).
     - **Priority 2** - Mid-level priority - Should be released at earlies convenience.
     - **Priority 3** - Low priority - Does not need to be released at the moment.

5. **Review and QA by Dev Lead:**  
   The project or dev lead reviews the pull request. If approved, they merge the feature branch into `develop`.  
   If there are a lot of commits and it makes more sense, combine them, squash and merge.

6. **Testing on Develop:**  
   After mergin the branch into `develop` and once build process has run, check `develop` site for testing/QA.
   - Make any database (wp-admin) changes to `develop` site.
   - If external review/approval is needed from other teams and/or the client, send them the `develop` link so they can test/review.
   - Apply any necessary changes/revisions, and re-push to `develop` branch, re-review as necessary.

7. **Feature Approved:**  
   Once the `develop` version is approved, the feature is scheduled for a production release (aka code release).

8. **Pre-Release Testing:**  
   During the next scheduled code release, the `develop` branch (which at this point likely includes multiple new 'feature' branches that have gone through the above workflow) will be first merged into the `staging` branch.  
   Changes pushed to `staging` will automatically be included in any releases to `production`.  
   Use the `staging` environment for final pre-release testing.  
   The project or web lead will creates the `pre-release`. Once a feature branch is merged into `staging`, the developer does one final round of QA/testing on `staging`.  
   - Make any database (wp-admin) changes to the `staging` site.

9. **Production Release Preparation:**  
   During the next scheduled code release, the `develop` branch will be merged into the `production` branch.  
   Changes to `production` are typically merged either on Tuesdays or Thursdays (ideally would prefer not to deploy on Friday, but depends on client needs).

10. **Code Release to Production:**  
   The `production` branch always reflects the live site.  
   Code releases involve deploying the changes from the production branch to the live site (`production-built`).  
   After a code release, the developers perform one final test on the live site to ensure everything is good.  
      - Make any database (wp-admin) changes to live site  

!> Never do Pull Requests/merges to the `production` branch from a feature branch (only reverting code or emergency fixes).

!> Do not commit the files in the `dist` folder. These will be built on merge for each environment branch.

## Branch Naming Convention

When you are working on a bug, feature, or improvement, you will need to create a branch.

Branch naming convention includes folders to keep our branches as organized as possible:

- `plugins/{plugin-name-slug-folder}/{feature-or-fix-name}` for plugin development
- `plugins/{plugin-name-slug-folder}` for 3rd party plugin updates
- `themes/{specific-theme-slug}/{feature-name}` for developing a feature which will specifically apply to one theme
- `themes/{feature-name-for-all-themes}` for developing a fix/feature that will apply to all the sites' themes

## Coding Flowchart

![Web Dev Workflow](./_images/../../_images/web-dev-workflow.svg)

## Committing

We encourage developers to write [meaningful commit messages](https://www.freecodecamp.org/news/how-to-write-better-git-commit-messages/).

### Contribution guidelines

#### Code quality

Follow the instructions from the [WPVIP documentation](https://docs.wpvip.com/how-tos/php_codesniffer/) about installing and using PHP CodeSniffer. This will get the correct standards installed.

VIP recommends integrating PHPCS inside code editors or IDEs to receive PHPCS feedback in real-time during development. For VS Code, [multiple](https://marketplace.visualstudio.com/search?term=phpcs&target=VSCode&category=All%20categories&sortBy=Relevance) plugins are available.

#### Style guide

[Stylelint](https://stylelint.io/), ESLint and prettier are used. ESLint's recommended rules are used for code-quality. Formatting is handled by prettier. If you have ESLint installed with your text editor of choice, you should be up and running.

#### Consider accessibility in UI changes

If the change you're proposing touches a user interface, include accessibility in your approach. This includes things like color contrast, keyboard accessibility, screen reader labels, and other common requirements. For more information, check out the Forem Accessibility docs page.

## Pull Request

Follow [these steps](local-development/preparing-pr.md) when preparing a pull request

### Pull request reviews

All pull requests should be are reviewed by the project lead or the person assign to this particular task.

- All required CI checks are expected to pass on each PR.
- In the case of flaky or unrelated test failures, a core team member will restart CI.
- Requested Changes must be resolved (with code or discussion) before merging.
- If you make changes to a PR, be sure to re-request a review.
- Style discussions are generally discouraged in PR reviews; make a PR to the linter configurations instead.
- Your code will be deployed shortly after it is merged.

## Final conclusion

We are a group of individuals working together to build outstanding websites for our clients and learn from each other. It's important to treat each other with kindness and acknowledge that compromises may need to be made.

----------------------------------------------------------------------

> Last Modified: {docsify-updated}
