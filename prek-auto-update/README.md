# Run `prek auto-update` and generate a PR for changes

```yaml
name: prek auto-update

on:
  schedule:
    - cron: "0 12 * * 5" # Every Friday at 12:00 UTC (5am PDT / 4am PST).
  workflow_dispatch:

permissions: {}

jobs:
  prek-auto-update:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    permissions:
      contents: write # Change files
      pull-requests: write # Create and update PRs
    steps:
      - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
        with:
          # zizmor: ignore[artipacked] no archive; needed for git
          persist-credentials: true
      - uses: danielparks/github-actions/prek-auto-update@2f6c98c73f0a8130d737500b59a0fddf749b484d # v1.1.1
        with:
          token: ${{ secrets.AUTO_UPDATE_TOKEN }}
```

You must either:

- Supply a token with **write** access to **contents** and **pull requests**. PRs created this way will run PR checks normally.
- Enable “Allow GitHub Actions to create and approve pull requests” (In your repo under Settings → Actions → General). However, PRs created this way will not run PR checks.

  You can enable “Allow GitHub Actions to create and approve pull requests” via the command line with:

      gh api --method PUT '/repos/{owner}/{repo}/actions/permissions/workflow' \
        --field can_approve_pull_request_reviews=true
