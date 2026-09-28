# YouTube Transcript Cursor Plugin

[![License](https://img.shields.io/badge/License-MIT-4CAF50?style=for-the-badge)](./LICENSE)
[![Website](https://img.shields.io/badge/Website-getyoutubetranscript.com-FF3B00?style=for-the-badge)](https://getyoutubetranscript.com)

A [Cursor Plugin](https://cursor.com/docs/plugins) that gives your Cursor agent YouTube transcripts, video/channel search, channel browsing, and playlist extraction - no local server to run, no Google API key or quota to manage. It bundles the hosted [`getyoutubetranscript.com`](https://getyoutubetranscript.com) MCP server plus skills, slash commands, rules, and a research agent so the setup and the conventions ship together.

**Free tier - 100 credits on signup, no card required.**

---

## Install

Once the plugin is listed on the Cursor marketplace (submitted, pending review):

```
/add-plugin youtube-transcript-cursor-plugin
```

or **Customize** in the sidebar -> search for **YouTube Transcript** -> **Install** (choose project or user scope).

Right now, install straight from this repo: use **Dashboard -> Plugins & MCPs -> Team Marketplaces -> Add Marketplace -> Import from Repo** and paste this repository's URL, or clone it into `~/.cursor/plugins/local/youtube-transcript-cursor-plugin/`.

---

## Setup

The bundled MCP server (`https://getyoutubetranscript.com/api/mcp`) authenticates with an API key:

1. **API key.** Get a free key (100 credits, no card) at the [dashboard](https://getyoutubetranscript.com/dashboard), or just ask your agent for a transcript with no key configured - the `fetch-transcript` skill can sign you up by email + one-time code without you leaving the conversation. Set the key as this plugin's `YOUTUBE_TRANSCRIPT_API_KEY` variable (Cursor prompts for it on install, or set it later in plugin settings).
2. **OAuth (alternative).** The server also supports OAuth. If you'd rather not manage a key, add `https://getyoutubetranscript.com/api/mcp` directly in Cursor's MCP settings without a header and approve the consent screen when Cursor prompts for sign-in.

---

## Components

### Skills

| Skill | Description |
|:------|:------------|
| [`fetch-transcript`](skills/fetch-transcript/SKILL.md) | Fetch a video's transcript plus title/author/thumbnail |
| [`search-youtube`](skills/search-youtube/SKILL.md) | Search YouTube for videos or channels |
| [`channel-playlist-research`](skills/channel-playlist-research/SKILL.md) | Resolve a handle, browse a channel's uploads, search within a channel, or list a playlist |
| [`summarize-video`](skills/summarize-video/SKILL.md) | Summarize or repurpose one or more videos (composes the skills above) |

### Commands

| Command | Description |
|:--------|:------------|
| [`/youtube-transcript`](commands/youtube-transcript.md) | Fetch a video's transcript by URL or ID |
| [`/youtube-search`](commands/youtube-search.md) | Search YouTube for videos or channels |
| [`/youtube-channel`](commands/youtube-channel.md) | Browse or search a channel's uploads |

### Rules

| Rule | Description |
|:-----|:------------|
| [`api-key-setup`](rules/api-key-setup.mdc) | Never commit keys; API key vs. OAuth setup for the bundled MCP server |
| [`transcript-conventions`](rules/transcript-conventions.mdc) | Untrusted-content handling, pagination, credits, error reporting |

### Agent

| Agent | Description |
|:------|:------------|
| [`youtube-research-specialist`](agents/youtube-research-specialist.md) | Routes any YouTube research task to the right skill and tool |

### MCP server

Bundled via [`mcp.json`](./mcp.json) - the remote [`tubeagentkit/youtube-mcp`](https://github.com/tubeagentkit/youtube-mcp) server, exposing `get_youtube_transcript`, `search_youtube`, `get_channel_latest_videos`, `search_channel_videos`, `list_channel_videos`, `list_playlist_videos`, and `get_credits`.

---

## Usage

Just install and ask - no config, no code.

```
Summarize this video: https://youtu.be/dQw4w9WgXcQ
Find videos about machine learning
Search MKBHD's channel for iPhone reviews
List every video @TED has ever uploaded
Get transcripts for every video in this playlist: [URL]
```

Or use the slash commands directly:

```
/youtube-transcript https://www.youtube.com/watch?v=jNQXAC9IVRw
/youtube-search machine learning explainers
/youtube-channel @veritasium --all
```

---

## Pricing

| Plan | Price | Credits | Rate Limit |
|---|---|---|---|
| **Free** | $0 | 100 credits on signup | 60 req/min |
| **Monthly** | $5/month | 1,000 credits/month | 200 req/min |
| **Annual** | $4.50/mo ($54/yr) | 1,000 credits/month | 300 req/min |

1 credit = 1 successful request. Failed calls are never charged. `get_channel_latest_videos`, `resolve`, and `get_credits` are always free. [Manage billing ->](https://getyoutubetranscript.com/dashboard)

---

## Related projects

Other ways to use the [GetYouTubeTranscript API](https://getyoutubetranscript.com):

- [youtube-mcp](https://github.com/tubeagentkit/youtube-mcp) - the remote MCP server this plugin bundles, for Claude, ChatGPT, Cursor, and VS Code directly
- [youtube-transcript-skills](https://github.com/tubeagentkit/youtube-transcript-skills) - the same functionality as a portable Agent Skill for Claude Code, Codex, and any Agent Skills-compatible tool
- [youtube-transcript-api-python](https://github.com/tubeagentkit/youtube-transcript-api-python) - Python SDK
- [youtube-transcript-api-node](https://github.com/tubeagentkit/youtube-transcript-api-node) - Node.js / TypeScript SDK
- [youtube-transcript-api](https://github.com/tubeagentkit) - the underlying REST API

## Disclosure

getyoutubetranscript.com is an independent product and is not affiliated with or endorsed by YouTube or Google.

## Contributing

Issues and PRs welcome.

## License

MIT - see [LICENSE](./LICENSE).
