# test-pi-coding-agent-action

Testing repo to help test and develop [shaftoe/pi-coding-agent-action](https://github.com/shaftoe/pi-coding-agent-action) GitHub Action.

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       ├── comment.yml   # Issue/PR comment trigger for the Pi agent
│       ├── pi.yml        # Reusable workflow – core Pi agent job
│       └── pr.yml        # Reusable workflow – PR review with Pi agent
├── .gitignore
├── LICENSE               # MIT License © 2026 Alexander Fortin
└── README.md
```

## Workflows

| Workflow | File | Trigger | Description |
|---|---|---|---|
| **Pi AI Agent** | `comment.yml` | `issue_comment`, `pull_request_review`, `pull_request_review_comment` | Detects `/pi` commands in issue/PR comments and dispatches the Pi agent |
| **Pi Agent** | `pi.yml` | `workflow_call` (reusable) | Shared job that runs the Pi agent; used by both `comment.yml` and `pr.yml` |
| **Review PR** | `pr.yml` | `workflow_call` (reusable) | Dedicated PR review workflow; checks out the PR branch and runs the Pi agent with a review-focused prompt and the `pi-librarian` extension |

### Workflow Details

#### `comment.yml` – Pi AI Agent (entry point)

Listens for new comments on issues and PRs. When a comment starts with `/pi`, it calls `pi.yml` with the `allowed_actor` input (set to `shaftoe`). Only the allowed actor may trigger the agent.

#### `pi.yml` – Pi Agent (reusable core)

The shared reusable workflow that:

1. Checks out the repository (PR head ref when applicable, with full history)
2. Sets up Node.js 24
3. Runs `shaftoe/pi-coding-agent-action@dev` with the configured provider, model, and thinking level
4. Uploads the session HTML as an artifact when available

#### `pr.yml` – Review PR (reusable)

A dedicated PR review workflow that:

1. Checks out the PR branch with full history
2. Sets up Node.js 24 and Bun
3. Runs the Pi agent with a review-focused prompt: *"This is a GitHub PR that needs code review. Focus on identifying issues, bugs, security concerns, and improvements."*
4. Enables the `pi-librarian` extension for enhanced review capabilities

## Usage

- Type `/pi <instruction>` in any **issue comment**, **PR review**, or **PR review comment** to invoke the agent.
- Example: `/pi review this PR` — triggers a code review of the current PR.
- Access control is enforced via the `allowed_actor` input; only the configured GitHub user can trigger the agent.

## Configuration

### Required Secrets

| Secret | Purpose |
|---|---|
| `LLM_API_KEY` | API key for the LLM provider |
| `GITHUB_TOKEN` | Automatically provided by GitHub Actions |

### Required Repository Variables

| Variable | Purpose |
|---|---|
| `PROVIDER` | LLM provider name (e.g. `openai`, `anthropic`) |
| `MODEL` | LLM model identifier (e.g. `gpt-4o`, `claude-sonnet-4-20250514`) |
| `THINKING_LEVEL` | Agent thinking/reasoning level |

### Permissions

All workflows require the following permissions:

| Permission | Access |
|---|---|
| `contents` | write |
| `issues` | write |
| `pull-requests` | write |

## License

This project is licensed under the [MIT License](LICENSE) © 2026 Alexander Fortin.
