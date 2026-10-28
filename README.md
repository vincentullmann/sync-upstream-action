# Sync upstream

Reusable workflow that keeps a fork's branch as its parent's branch of the same name,
plus the commits this fork has on top. The branch is `default` unless the workflow
passes `branch`.

## One-time setup

```sh

# fork and clone the repo
gh repo fork ynput/ayon-nuke --org straylondon --clone


# add the action
nano .github/workflows/sync-upstream.yaml
git add .github/workflows/sync-upstream.yaml
git commit -m "ci: added sync-upstream"


# optional: add other bits
nano client/pyproject.toml
git add client/pyproject.toml
git commit -m "fixed pyproject toml"

# push
git push -u
```

## Example

```yaml
# .github/workflows/sync-upstream.yaml
name: Sync upstream

permissions:
  contents: write


on:
  schedule:
    - cron: "0 6 * * 1-5"  # Mo-Fr 06:00
  workflow_dispatch:

permissions:
  contents: write

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: vincentullmann/sync-upstream@main
        with:
          branch: develop
```
