# claude-runner

A GitHub Actions workflow that keeps your Claude usage timer optimally reset.

## Why?

Claude subscriptions have a token limit that resets every 5 hours—but the clock starts when _you_ use it. If you exhaust your usage at 8am, it won't reset until 1pm.

This workflow pings Claude every 5 hours automatically, so the reset timer is always running. Your 8am usage now resets at 10am (from the 5am ping) instead of 1pm.

## Setup

1. **Fork this repository**

2. **Generate an OAuth token** using [Claude Code](https://docs.anthropic.com/en/docs/claude-code) locally:

   ```bash
   claude setup-token
   ```

   This will output a token for CI/CD use.

3. **Add the token to your fork** at `Settings → Secrets and variables → Actions → New repository secret`:
   - Name: `CLAUDE_CODE_OAUTH_TOKEN`
   - Value: the token from step 2

4. **Enable Actions** on your fork if prompted

The workflow will now run hourly, pinging Claude whenever 5+ hours have passed since the last ping.

## How it works

The workflow runs every hour but only actually calls Claude if 5+ hours have elapsed since the last successful run. State is stored in GitHub Actions cache to track timing between runs.

## Customization

Edit `.github/workflows/claude.yml` to adjust:

- `cron` schedule (default: hourly checks)
- `18000` seconds threshold (default: 5 hours)
- `prompt` sent to Claude
- `--model` flag (default: claude-haiku-4-5)
