# Merge Master

automatically merge default branch to topic branch or rebase renovate bot PRs.

When

- CI complete
- PR is enabled auto merge
- default branch is updated

Then

- Merge default branch to a branch that is enabled auto merge
- Rebase renovate bot PRs.

If there is a PR that is running CI and following the default branch, Marge Master will not merge the default branch to the PR branch.

## 👏 Usage

Github Personal Access Token or Github App Token is required to use this action because a commit created via default Github Action Token cannot trigger another Github Action.

The Token or App must have the following permissions:

- checks: read
- metadata: read
- pull_requests: write
- issues: write
- code: write

### With Personal Access Token

```yaml
name: Merge Master

on:
  workflow_run:
    workflows:
      - "{The slowest CI Names}"
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
      - uses: praha-inc/merge-master@v1
        with:
          github-token: ${{ secrets.GITHUB_PAT_TOKEN }}
```

### With Github App Token

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
      - uses: praha-inc/merge-master@v1
        with:
          github-token: ${{ steps.get-app-token.outputs.token }}
```

## 🤝 Contributing

Contributions, issues and feature requests are welcome.

Feel free to check [issues page](https://github.com/praha-inc/merge-master/issues) if you want to contribute.

## 📝 License

Copyright © 2020 [PrAha](https://www.praha-inc.com/).

This project is [```MIT```](https://github.com/praha-inc/merge-master/blob/main/LICENSE) licensed.
