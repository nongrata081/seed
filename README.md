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

**Recommended flow:**

1. Run `yarn recommended:bump` to find out what is the recommended version for bump
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

----
According to gitflow, you first branch out to a new branch (e.g. `feature/some-feature`) to work on some functionality.
While developing on that branch, you might make multiple commits. Since changelog is generated based on existing conventional
commits (of types: `feat`, `fix`, `perf`, `revert`, `revert`), one has to shape the commit-history in such a way, that the 
resulting generated changelog on one-hand is clean (has no duplicate data as would be in case of having multiple conventional
commits for one feature in a single branch), and on the other hand contains all the necessary data (a separate bullet in the
changelog list which you want to be present there).

Your tools to achieve that are:

- **Squash commits** into a single commit, thus have a single bullet in the changelog generated from it.
- Keep **separate commits** for the functionality you want to be present as bullets in the changelog
---

**Why squash commits?**

Squashing related commits is considered a good practice, since it helps to keep the git history clean
(especially valuable when there are a lot of people developing and pushing the code) and prevent its
pollution with commits (e.g. an enterprise project with tens of developers making and pushing commits
daily, without squashing very soon it will become hard to reason about the contents of git log due to
enormous amount of commits).

Let's say you branched out to a new feature branch and developed a feature, and made several commits:
![feature-several-commits](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/77d62919f24fdf664f7fb43ba5cecf27/Screenshot_2019-04-02_at_12.49.04.png)

If you would then generate a changelog without squashing related commits, you would get a bloated changelog like this:
![bloated-changelog](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/81185d451ef18c1495c830161b3f21ee/Screenshot_2019-04-02_at_12.52.52.png)

Now imagine, if you have 10 or more commits per feature? And then imagine, if you have 10 features?
And then imagine, if you have 10 developers, each pushing 10 features? At the end of the day you have
1000 commits for a release contributed to by 10 developers, polluted git history and changelog and difficult
times with reasoning about `git log` if you have to. 

At this step, `squashing commits` comes to rescue.

**How to squash commits?**

In order to squash commits run `git rebase -i HEAD~N`, where `N` stands for amount of last commits, which
you want to be able to perform actions on in rebase editor (squash, edit, etc).
In this case we would like to squash last 5 commits, so run `git rebase -i HEAD~5`, which opens git rebase editor : 
![rebase-commits-editor](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/a0d8afdd24b046ce0e7cbb816043cdb9/Screenshot_2019-04-02_at_13.41.16.png)

We would like to squash all feature functionality-specific commits into a single generic commit, so we leave `pick`
for commit we want to keep, and replace word `pick` with `squash` for commits we want to squash:

![squash-commits](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/07920c256909e45c6a1ff71e571ae0c6/Screenshot_2019-04-02_at_13.52.57.png)

Then write:quit and we get combination of squashed commits (where we can either edit or comment certain commit messages, if needed):

![combined-commits](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/4baa3b41e37edc912a60131ac184d10b/Screenshot_2019-04-02_at_14.00.14.png)

Then write:quit again and we get:

![squashed-commits](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/5252d47e8e1e1341c72d6089b269e9fd/Screenshot_2019-04-02_at_14.06.48.png)

At this step our last 5 commits were squashed into a single commit, what we can see from `git log`:

![squashed-commit](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/e4de9fd4389840ae590268bae9675c42/Screenshot_2019-04-02_at_14.09.06.png)

Now, if we generate a changelog, it will have just one generic feature commit, into which we squashed other feature-specific commits:

![clean-changelog](https://trello-attachments.s3.amazonaws.com/5c544e9a28eba76fabf3dedb/5c54672549044a1837c0364c/1d36b6991f306a114aee5283f2eed4bc/Screenshot_2019-04-02_at_14.19.09.png)

So as a result you get a clean changelog without duplicate data, yet at the same time you have more details about which commits does the generic one contain in `git-log`

---


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
