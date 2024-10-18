# Merge Master

automatically merge main branch to topic branch or rebase renovate bot PRs.

When
- CI is completed
- PR is enabled auto merge
- main branch is updated

Then
- Merge main branch to a branch that is enabled auto merge
- Rebase renovate bot PRs.

If there is a PR that is running CI and following the main branch, Marge Master will not merge the main branch to the PR branch.


# 👏 Usage

Github Personal Access Token or Github App Token is required to use this action because a commit created via default Github Action Token cannot trigger another Github Action.

## With Personal Access Token

```yaml
name: Merge Master

on:
  workflow_run:
    workflows:
      - "{Your CI Names}"
    types:
      - completed
  pull_request:
    types:
      - auto_merge_enabled
  push:
    branches:
      - main
concurrency:
  group: merge-master

jobs:
  merge-master:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/create-github-app-token@5d869da34e18e7287c1daad50e0b8ea0f506ce69 # v1.11.0
        id: app-token
        with:
          app-id: ${{ vars.AUTO_UPDATE_BRANCH_BOT_APP_ID }}
          private-key: ${{ secrets.AUTO_UPDATE_BRANCH_BOT_PRIVATE_KEY }}
      - uses: agaroot-technologies/merge-master@v1.0.0
        with:
          github-token: ${{ secrets.GITHUB_PAT_TOKEN }}
```

## With Github App Token

The Github App is required to have the following permissions:


```yaml
name: Merge Master

on:
  workflow_run:
    workflows:
      - "{Your CI Names}"
    types:
      - completed
  pull_request:
    types:
      - auto_merge_enabled
  push:
    branches:
      - main
concurrency:
  group: merge-master

jobs:
  merge-master:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/create-github-app-token@5d869da34e18e7287c1daad50e0b8ea0f506ce69 # v1.11.0
        id: get-app-token
        with:
          app-id: ${{ vars.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}
      - uses: agaroot-technologies/merge-master@main
        with:
          github-token: ${{ steps.get-app-token.outputs.token }}
```



## 🤝 Contributing

Contributions, issues and feature requests are welcome.

Feel free to check [issues page](https://github.com/agaroot-technologies/merge-master/issues) if you want to contribute.

## 📝 License

Copyright © 2020 [AGAROOT TECHNOLOGIES](https://tech.agaroot.co.jp/).

This project is [```MIT```](https://github.com/agaroot-technologies/merge-master/blob/main/LICENSE) licensed.
