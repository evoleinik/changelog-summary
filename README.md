# Changelog Summary

Summarize git commits with AI and post to Slack. No fluff, just what shipped.

## Usage

```yaml
name: Changelog Summary

on:
  schedule:
    - cron: '0 13 * * *'  # Daily at 1 PM UTC
  workflow_dispatch:

jobs:
  summary:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Need full history

      - uses: evoleinik/changelog-summary@v1
        with:
          slack-webhook: ${{ secrets.SLACK_WEBHOOK_URL }}
          llm-provider: gemini
          llm-api-key: ${{ secrets.GEMINI_API_KEY }}
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `slack-webhook` | Yes | - | Slack incoming webhook URL |
| `llm-provider` | No | `gemini` | AI provider: `gemini`, `openai`, or `anthropic` |
| `llm-api-key` | Yes | - | API key for the LLM provider |
| `since` | No | `24 hours ago` | Time range for commits |
| `header` | No | `Daily Update` | Header text (e.g., "Weekly Update") |
| `voice` | No | `founder` | Summary style: `founder`, `developer`, or `marketing` |
| `project-name` | No | repo name | Project name in header |

## Voice Styles

**founder** - Direct, no-BS summary of what shipped. Cuts through the noise.

**developer** - Technical focus: APIs, breaking changes, bug fixes. Specific about what changed.

**marketing** - User-facing improvements and new capabilities. Positive but grounded.

## Weekly Summary

```yaml
on:
  schedule:
    - cron: '0 13 * * 0'  # Sundays at 1 PM UTC

jobs:
  summary:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: evoleinik/changelog-summary@v1
        with:
          slack-webhook: ${{ secrets.SLACK_WEBHOOK_URL }}
          llm-provider: gemini
          llm-api-key: ${{ secrets.GEMINI_API_KEY }}
          since: '7 days ago'
          header: 'Weekly Update'
```

## LLM Providers

### Gemini (default)
- Model: `gemini-3-flash-preview`
- Get API key: [Google AI Studio](https://aistudio.google.com/apikey)

### OpenAI
- Model: `gpt-4o-mini`
- Get API key: [OpenAI Platform](https://platform.openai.com/api-keys)

### Anthropic
- Model: `claude-sonnet-4-20250514`
- Get API key: [Anthropic Console](https://console.anthropic.com/)

## Example Output

> **MyProject Daily Update (Dec 22)**
>
> • Shipped *multi-provider dashboard* with real-time sync
> • Fixed authentication bug causing logout loops
> • Added CSV export for analytics data
> • Improved search performance by 3x
>
> _12 commits by 2 contributor(s)_

## License

MIT
