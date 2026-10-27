# Changelog

## Unreleased

- The bundled MCP server gained `list_channel_playlists`, `list_channel_posts`, `get_channel_banner`, `get_related_videos` and `get_video_dislikes` (1 credit each), and `list_channel_videos` takes `tab` (`videos`, `shorts`, `streams`) and `sort` (`latest`, `popular`, `oldest`). `channel-playlist-research` skill, agent and README describe them
- The bundled MCP server now has `get_transcript_languages` (1 credit): lists the caption languages a video offers. `fetch-transcript` skill and README mention it

## 1.0.0 (2026-09-28)

- Initial release
- Bundled `youtube-transcript` remote MCP server (`https://getyoutubetranscript.com/api/mcp`) - API key auth (OAuth also supported by the server)
- Skills: `fetch-transcript`, `search-youtube`, `channel-playlist-research`, `summarize-video`
- Commands: `/youtube-transcript`, `/youtube-search`, `/youtube-channel`
- Rules: `api-key-setup`, `transcript-conventions`
- Agent: `youtube-research-specialist`
