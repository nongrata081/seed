# seed

Purpose of this repo is to serve as a seed repository for any kind of project, that has some
basic functionality:

# Commit linting
Checks if your commit messages meet the conventional commit format. Note: Please check [rules](https://github.com/conventional-changelog/commitlint/blob/master/docs/reference-rules.md) / [rules](https://conventional-changelog.github.io/commitlint/#/reference-rules)  before using it.

In general the pattern mostly looks like this:

`type(scope?): subject` (scope is optional)

Real world examples can look like this:

`chore: run tests on travis ci`

`fix(server): send cors headers`

`feat(blog): add comment section`

---

Common types according to [commitlint-config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional#type-enum) (based on the the Angular convention) can be:

- build
- ci
- chore
- docs
- feat
- fix
- perf
- refactor
- revert
- style
- test

These can be modified by your own [configuration](https://github.com/conventional-changelog/commitlint#config).

You still can commit with `git commit`, however the recommended way to do it is by using **cli-prompt** for commits.

Implemented with [commitlint](https://github.com/conventional-changelog/commitlint)

![](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c544f7796142c1efdc3edd0/f8c47b856c62436587298256cff0ae77/Screen_Shot_2018-12-05_at_11.06.23.png)

---

# CLI prompt for commits

You still can commit with git commit, however the recommended way to do it is by using cli-prompt for commits - commitizen-cli. Run `yarn commit` to open 
a prompt with a step-by-step wizard with hints to properly fill in the commit data.

![](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c544f7796142c1efdc3edd0/b7a3e5a846f7b88247d8e5da3285e81f/Screen_Shot_2018-12-05_at_11.23.31.png)

Implemented with [commitizen](https://github.com/commitizen/cz-cli)

Following conventional commit format will allow us to automatically generate changelog from commit history, that would have chapters (features, bugfixes, etc.) and would look like [this](https://github.com/angular/material2/releases)

# Recommended version bump
Run `yarn recommended:bump` to find out what is the reccomended version for bump.

Implemented with [recommended bump](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-recommended-bump)

# Changelog generation
Run `yarn changelog` to generate a changelog (will overwrite any previous changelog if exists). Generates a changelog based on commits since the last semver tag that match the pattern of a "Feature", "Fix", "Performance Improvement" or "Breaking Changes". Conventional commits are a necessary prerequisite, enabling automatic changelog generation from commit history.

Implemented with [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog).

![](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/aff687d86a74b45704cb8556ce242452/2019-02-01_16-48-48.png)

---
Recommded flow:

1. Run `yarn recommended:bump` to find out what is the reccomended version for bump
2. Depending on which version you want to bump, run one of the following (this will also create git tag):
    ```	
    yarn version --major 
    yarn version --minor
    yarn version --patch
    ```
3. Generate changelog with `yarn changelog`
4. Commit `package.json` and `CHANGELOG.md` files
5. Push including tags with `git push --follow-tags`
6. Manually create a release draft in Github and copypaste changelog’s release markdown to it

# Enforce gitflow branches
Implemented with [enforce-gitflow-branches](https://github.com/Dacrol/EnforceBranchNames)

![](https://trello-attachments.s3.amazonaws.com/5c546d7eddb4a82f52f14917/970x1572/915f284c485422b5aab05baa832bbb8b/image.png)

# Enforce yarn
Programmatically enforce usage of yarn instead of npm for installing node packages.
Implemented with `.nvmrc` according [to](https://github.com/yarnpkg/yarn/issues/4895).
![](https://trello-attachments.s3.amazonaws.com/5c55aeddd546a91ee331ecea/1028x390/99d9274660a0ab1de5ab01cb492dbf99/image.png)

![](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c55aeddd546a91ee331ecea/d4682258d34fce791634528d27997cec/image.png)

# Enforce node version (.nvmrc)
Implemented according [to](https://medium.com/@hebet/locking-down-a-project-to-a-specific-node-version-using-nvmrc-and-or-engines-e5fd19144245)

# Editorconfig
Implemented with [editorconfig](https://editorconfig.org/)
