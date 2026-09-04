---
engine: copilot
name: Weekly Report Status
description: Publish a concise weekly activity report for the repository.
intent: Summarize the repository's activity during the previous seven full UTC days in a new issue.
on:
  schedule:
    - cron: "0 9 * * 1"
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
safe-outputs:
  create-issue:
    title-prefix: "[weekly-report] "
    max: 1
---

# Weekly Report Status

## Task

Create a concise activity report for the previous seven full days ending at the workflow start time in UTC. Review repository commits, issues, and pull requests during that window.

The report must:

- State the UTC reporting window and workflow trigger.
- Summarize the activity with clear counts for commits, issues, and pull requests.
- Highlight the most relevant changes, discussions, or completed work.
- State clearly that no activity occurred when all three categories are empty.
- Follow the report structure: `### Summary`, visible critical information, optional `<details>` sections for itemized details, and `### Context`.
- Include up to three relevant workflow run references at the end when available.

Use GitHub read tools to gather evidence. Do not infer activity that is not supported by repository data.

## Safe Outputs

- When the report is supported by the collected evidence, publish it with the configured `create-issue` safe output using a concise descriptive title.
- If there are no commits, issues, or pull requests in the reporting window, call `noop` with the exact evaluated UTC window and a short explanation instead of creating an issue.
- Call `noop` with a short explanation if the repository data is unavailable or insufficient to produce an accurate report.
