# conventional-multiplatform-lib

Ready to use example of Kotlin Multi Platform library with
[Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/#specification) release flow.

## Development & release process

General rules:

* All changes to `master` branch MUST be merged using Pull Requests (PRs)
  * By default, each PR is rebased from `master` and all commits are squashed
    * If required (e.g. bug fix, new feature and docs update done in one PR), commits can be split
    * In this case each of the commits MUST correspond to Conventional Commits Specification
* All commits that will be merged in `master` MUST correspond to the Conventional Commits Specification
  * Individual commits to the branch can not correspond to the Conventional Commits Specification
* Releases are generated automatically with each merge to `master`

> Please note: it does not matter which Git Workflow is in use in the repository.<br>
> This solution works with any kind of Git Workflow and can be easily adopted further
> to generate pre-releases, alpha/beta versions, etc.

> test commit

> test commit 2

### Semantic release

After studying of multiple available solutions to automate release process for any programming language and build system,
it was decided to use state-of-the-art open source [Semantic Release](https://semantic-release.gitbook.io/semantic-release/) tool.

Semantic Release Highlights:
* Fully automated release
* Enforce Semantic Versioning specification
* New features and fixes are immediately available to users
* Notify maintainers and users of new releases
* Use formalized commit message convention to document changes in the codebase
* Publish on different distribution channels (such as npm dist-tags) based on git merges
* Integrate with your continuous integration workflow
* Avoid potential errors associated with manual releases
* Support any package managers and languages via plugins
* Simple and reusable configuration via shareable configurations

It works as follows:

| Step              | Description                                                                     |
|-------------------|---------------------------------------------------------------------------------|
| Verify Conditions | Verify all the conditions to proceed with the release                           |
| Get last release  | Obtain the commit corresponding to the last release by analyzing                |
| Analyze commits   | Determine the type of release based on the commits added since the last release |
| Verify release    | Verify the release conformity                                                   |
| Generate notes    | Generate release notes for the commits added since the last release             |
| Create Git tag    | Create a Git tag corresponding to the new release version                       |
| Prepare           | Prepare the release                                                             |
| Publish           | Publish the release                                                             |
| Notify            | Notify of new releases or errors                                                |

### Automation and tools

* Each PR title is checked for Conventional Commits Specification
  * All checks must pass for PR to be merged
  * It forces the squashed commit to correspond to Conventional Commits Specification
* Release workflow with each merge to `master` branch
  * New library version is automatically identified from commits history
  * `CHANGELOG.md` is automatically generated for a new release and updated in the repository
  * New tag is created and pushed to the repo
  * New release is created
  * *optional* Slack notification is sent
* Conventional pre-commit hooks for local environment

## <a name="commit"></a> Commit Message Format

We have very precise rules over how our Git commit messages must be formatted.
This format leads to **easier to read commit history**.

Each commit message consists of a **header**, a **body**, and a **footer**.

```
<header>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

The `header` is mandatory and must conform to the [Commit Message Header](#commit-header) format.

The `body` is mandatory for all commits except for those of type "docs".
When the body is present it must be at least 20 characters long and must conform to the [Commit Message Body](#commit-body) format.

The `footer` is optional. The [Commit Message Footer](#commit-footer) format describes what the footer is used for and the structure it must have.


#### <a name="commit-header"></a>Commit Message Header

```
<type>(<scope>): <short summary>
  │       │             │
  │       │             └─⫸ Summary in present tense. Not capitalized. No period at the end.
  │       │
  │       └─⫸ Commit Scope: apollo|castor|pollux|mercury|pluto|athena|pistis
  │
  └─⫸ Commit Type: build|ci|docs|feat|fix|perf|refactor|test
```

The `<type>` and `<summary>` fields are mandatory, the `(<scope>)` field is optional.


##### Type

Must be one of the following:

* **build**: Changes that affect the build system or external dependencies
* **ci**: Changes to the CI configuration files and scripts
* **docs**: Documentation only changes
* **feat**: A new feature
* **fix**: A bug fix
* **perf**: A code change that improves performance
* **refactor**: A code change that neither fixes a bug nor adds a feature
* **test**: Adding missing tests or correcting existing tests


##### Scope
The scope should be the name of the affected module or building block
(as perceived by the person reading the changelog generated from commit messages).

The following is the list of supported scopes:

* `apollo`
* `castor`
* `pollux`
* `mercury`
* `pluto`
* `athena`
* `pistis`

##### Summary

Use the summary field to provide a succinct description of the change:

* use the imperative, present tense: "change" not "changed" nor "changes"
* don't capitalize the first letter
* no dot (.) at the end


#### <a name="commit-body"></a>Commit Message Body

Just as in the summary, use the imperative, present tense: "fix" not "fixed" nor "fixes".

Explain the motivation for the change in the commit message body. This commit message should explain _why_ you are making the change.
You can include a comparison of the previous behavior with the new behavior in order to illustrate the impact of the change.


#### <a name="commit-footer"></a>Commit Message Footer

The footer can contain information about breaking changes and deprecations and is also the place to reference GitHub issues, Jira tickets, and other PRs that this commit closes or is related to.
For example:

```
BREAKING CHANGE: <breaking change summary>
<BLANK LINE>
<breaking change description + migration instructions>
<BLANK LINE>
<BLANK LINE>
Fixes ATL-<issue number>
```

or

```
DEPRECATED: <what is deprecated>
<BLANK LINE>
<deprecation description + recommended update path>
<BLANK LINE>
<BLANK LINE>
Related to ATL-<issue number>
```

Breaking Change section should start with the phrase "BREAKING CHANGE: " followed by a summary of the breaking change, a blank line, and a detailed description of the breaking change that also includes migration instructions.

Similarly, a Deprecation section should start with "DEPRECATED: " followed by a short description of what is deprecated, a blank line, and a detailed description of the deprecation that also mentions the recommended update path.


### Revert commits

If the commit reverts a previous commit, it should begin with `revert: `, followed by the header of the reverted commit.

The content of the commit message body should contain:

- information about the SHA of the commit being reverted in the following format: `This reverts commit <SHA>`,
- a clear description of the reason for reverting the commit message.

## Local environment: pre-commit hooks

[Conventional pre-commit hook](https://github.com/compilerla/conventional-pre-commit])
can be used to check that all commits correspond to the Conventional Commits Specification. 

Initialization:
* Make sure pre-commit is [installed](https://pre-commit.com/#install).
* Run: `pre-commit install --hook-type commit-msg`
