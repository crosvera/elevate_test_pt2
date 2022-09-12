# Elevate DevOps test pt2

## The problem

Our code is organized such that some of the repositories are included as submodules in others. Since we update these dependencies regularly, it’s important to keep them updated in the
repositories that use them.
Given a submodule named content, please implement a solution to create a GitHub pull request which updates the version of that submodule to the latest commit on its main branch. Your solution should run on a regular schedule and also automatically add a set of GitHub users
as reviewers on the pull request.

## Solution

In order to accomplish this we rely on [`dependabot`](https://github.com/dependabot), a tool from GitHub that facilitates the dependecy updates in our repositories. To start with it, we need to create a configuration file called `dependabot.yaml` at the `.github` subdirectory.

```yaml
version: 2
updates:
  - package-ecosystem: "gitsubmodule"
    directory: "/"
    schedule:
      interval: "daily"
    reviewers:
      - "crosvera"
```

In this configuration file we can set the different dependencies update mechanisms, in this case we are using `gitsubmodule`. `dependabot` will check all submodules under the `directory`. You can set the frequency that `dependabot` can check for dependency updates using the [`schedule.interval` option](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file#scheduleinterval). `dependabot` will gather all the updates available for our dependencies and will push them as a PR, in order to add reviewers to that PR, we can set the `reviewers` option with the usernames of the GitHub accounts.