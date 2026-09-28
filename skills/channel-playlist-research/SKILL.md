---
name: channel-playlist-research
description: Use when the user wants to resolve a channel handle, browse a channel's recent or full upload history, search inside one channel's videos, or list every video in a playlist. Calls get_channel_latest_videos, search_channel_videos, list_channel_videos, and list_playlist_videos from the bundled getyoutubetranscript.com MCP server (REST API fallback available) - channel/playlist listing is free, search and full-history pagination cost credits.
---

# Channel & playlist research

Covers everything about a YouTube channel or playlist short of fetching an individual video's transcript (see the `fetch-transcript` skill for that).

## Trigger

- "What has @channel posted recently?" -> latest videos (free)
- "List every video @channel has ever uploaded" -> full upload history (paginated)
- "Search @channel's videos for X" -> in-channel search (paginated)
- "List all videos in this playlist" -> playlist contents (paginated)
- "What's this channel's ID / is this the right channel?" -> resolve a handle/URL to a canonical channel ID (free)

## Preferred path: the bundled MCP tools

If the `youtube-transcript` MCP server is connected, use whichever tool matches the ask:

| Tool | Use for | Cost |
|---|---|---|
| `get_channel_latest_videos({ channel })` | Channel metadata + its home tab's recent uploads | Free |
| `list_channel_videos({ channel \| continuation })` | Every video the channel has ever uploaded, paginated | 1 credit/page |
| `search_channel_videos({ channel, query \| continuation })` | Search within one channel's videos | 1 credit/page |
| `list_playlist_videos({ playlist \| continuation })` | Every video in a playlist | 1 credit/page |

`channel` accepts an `@handle`, a channel URL, or a `UC...` ID. `playlist` accepts a playlist URL or ID. For any of the paginated tools, pass the previous response's opaque continuation token back verbatim to `continuation` for the next page - never construct or decode it. A `null`/absent token means there are no more pages.

## Fallback: the REST API

Same base URL and auth as `fetch-transcript` (`https://getyoutubetranscript.com/api/v1`, `Authorization: Bearer $YOUTUBE_TRANSCRIPT_API_KEY`):

| Endpoint | Params | Cost |
|---|---|---|
| `GET /resolve?handle=@mkbhd` | `handle` (ID, URL, or @handle) | Free |
| `GET /channel/latest?channel=@mkbhd` | `channel` | Free |
| `GET /channel/videos?channel=@mkbhd` (or `&continuation=`) | `channel` or `continuation` | 1 credit/page |
| `GET /channel/search?channel=@mkbhd&q=iphone` (or `&continuation=`) | `channel`+`q`, or `continuation` | 1 credit/page |
| `GET /playlist?list=<id or URL>` (or `&continuation=`) | `list` or `continuation` | 1 credit/page |

See the `fetch-transcript` skill for how to obtain `YOUTUBE_TRANSCRIPT_API_KEY`, and for the shared error-code table (401/402/404/429/503).

## Which one to reach for

- Just want to know what's new? `get_channel_latest_videos` / `channel/latest` - it's free, use it before reaching for the paginated full-history tool.
- Need the complete history (for an archive, a database, or "has this channel ever covered X")? `list_channel_videos` / `channel/videos`.
- Only care about one topic within a channel? `search_channel_videos` / `channel/search` - cheaper than paginating the full history and filtering yourself.
- Before paginating a large channel or playlist, consider checking remaining balance first (`get_credits` tool, or `GET /credits`, both free) so a long pagination loop doesn't die partway through on a `402`.
