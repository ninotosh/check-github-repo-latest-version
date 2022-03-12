[![.github/workflows/test.yml](https://github.com/ninotosh/check-github-repo-latest-version/actions/workflows/test.yml/badge.svg?branch=main)](https://github.com/ninotosh/check-github-repo-latest-version/actions/workflows/test.yml)
[![.github/workflows/check.yml](https://github.com/ninotosh/check-github-repo-latest-version/actions/workflows/check.yml/badge.svg)](https://github.com/ninotosh/check-github-repo-latest-version/actions/workflows/check.yml)

# check-github-repo-latest-version

GitHub Action to check if your application is using the latest versions of GitHub repositories.

This action would be useful if this is run [on schedule](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule).

## Usage

Either `release` or `tag` is required.

`github_token` is required even for accessing public repositories.
If you are afraid to use `${{ secrets.GITHUB_TOKEN }}`,
you can use another token with no permissions.

The action succeeds if the latest release starts with the input `release`
or the latest tag starts with the input `tag`.

Note that `v1` does not match `v10.2.3`, for example.

| input `release` | latest release | succeed? |
| --- | --- | --- |
| v1 | v1.2.3 | ✅ |
| v1.2 | v1.2.3 | ✅ |
| v1.2.3 | v1.2.3 | ✅ |
| v1.3 | v1.2.3 | ❌ |
| v1 | v10.2.3 | ❌ |
| v1.2 | v1.21.3 | ❌ |

## Example

[.github/workflows/check.yml](.github/workflows/check.yml)

```yml
name: check latest versions
on:
  workflow_dispatch:
  schedule:
    - cron: '13 0 * * *'

jobs:
  check:
    runs-on: ubuntu-20.04
    timeout-minutes: 1

    steps:
      - name: check the latest version of https://github.com/actions/checkout
        uses: ninotosh/check-github-repo-latest-version@v1
        with:
          release: actions/checkout@v3
          github_token: ${{ secrets.GITHUB_TOKEN }}
```
