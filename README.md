# test-pi-coding-agent-action

Testing repo to help test and develop [shaftoe/pi-coding-agent-action](https://github.com/shaftoe/pi-coding-agent-action) GitHub Action.

## Workflows

| Workflow | Trigger | Description |
|---|---|---|
| **Pi AI Agent** (`comment.yml`) | Issue/PR comment or review containing `/pi` | Dispatches the Pi agent to act on the comment instruction |
| **Review PR** (`pr.yml`) | Reusable (`workflow_call`) | Shared job that runs the Pi agent for PR review |
| **Pi Agent** (`pi.yml`) | Reusable (`workflow_call`) | Shared job that runs the Pi agent for ad-hoc tasks |

## Usage

- Type `/pi <instruction>` in any issue comment or PR review to invoke the agent.
- PR reviews can be triggered manually by invoking `/pi review this PR` in a PR comment.

### Required secrets

| Secret | Purpose |
|---|---|
| `LLM_API_KEY` | API key for the LLM provider |
| `GITHUB_TOKEN` | Automatically provided by GitHub Actions |

### Required variables

| Variable | Purpose |
|---|---|
| `PROVIDER` | LLM provider name |
| `MODEL` | LLM model identifier |
| `THINKING_LEVEL` | Agent thinking/reasoning level |
