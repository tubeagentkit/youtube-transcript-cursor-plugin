---
name: channel-playlist-research
description: Use when the user wants to resolve a channel handle, browse a channel's recent or full upload history (including Shorts, live streams or its most popular videos), see its playlists, community posts or banner, search inside one channel's videos, list every video in a playlist, find videos related to one, or read a video's estimated dislikes. Calls get_channel_latest_videos, search_channel_videos, list_channel_videos, list_channel_playlists, list_channel_posts, get_channel_banner, list_playlist_videos, get_related_videos and get_video_dislikes from the bundled getyoutubetranscript.com MCP server (REST API fallback available) - every call costs 1 credit per successful call (per page when paginated); only `get_credits` is free.
---

# Channel & playlist research

Covers everything about a YouTube channel or playlist short of fetching an individual video's transcript (see the `fetch-transcript` skill for that).

## Trigger

- "What has @channel posted recently?" -> latest videos (1 credit)
- "List every video @channel has ever uploaded" -> full upload history (paginated)
- "Search @channel's videos for X" -> in-channel search (paginated)
- "List all videos in this playlist" -> playlist contents (paginated)
- "What's this channel's ID / is this the right channel?" -> resolve a handle/URL to a canonical channel ID (1 credit)

## Preferred path: the bundled MCP tools

If the `youtube-transcript` MCP server is connected, use whichever tool matches the ask:

| Tool | Use for | Cost |
|---|---|---|
| `get_channel_latest_videos({ channel })` | Channel metadata + its home tab's recent uploads | 1 credit |
| `list_channel_videos({ channel \| continuation, tab, sort })` | The channel's uploads, paginated. `tab`: `videos` (default), `shorts`, `streams` (Live). `sort`: `latest` (default), `popular`, `oldest`. Both first page only | 1 credit/page |
| `list_channel_playlists({ channel \| continuation })` | The channel's playlists (pass an `id` to `list_playlist_videos`) | 1 credit/page |
| `list_channel_posts({ channel \| continuation })` | The channel's community posts | 1 credit/page |
| `get_channel_banner({ channel })` | Banner (original and every crop size) and avatar | 1 credit |
| `get_related_videos({ video_url })` | The videos YouTube suggests next to a video | 1 credit |
| `get_video_dislikes({ video_url })` | Estimated likes and dislikes (YouTube no longer publishes dislikes) | 1 credit |
| `search_channel_videos({ channel, query \| continuation })` | Search within one channel's videos | 1 credit/page |
| `list_playlist_videos({ playlist \| continuation })` | Every video in a playlist | 1 credit/page |

`channel` accepts an `@handle`, a channel URL, or a `UC...` ID. `playlist` accepts a playlist URL or ID. For any of the paginated tools, pass the previous response's opaque continuation token back verbatim to `continuation` for the next page - never construct or decode it. A `null`/absent token means there are no more pages.

## Fallback: the REST API

Same base URL and auth as `fetch-transcript` (`https://getyoutubetranscript.com/api/v1`, `Authorization: Bearer $YOUTUBE_TRANSCRIPT_API_KEY`):

| Endpoint | Params | Cost |
|---|---|---|
| `GET /resolve?handle=@mkbhd` | `handle` (ID, URL, or @handle) | 1 credit |
| `GET /channel/latest?channel=@mkbhd` | `channel` | 1 credit |
| `GET /channel/videos?channel=@mkbhd` (or `&continuation=`) | `channel` or `continuation`; optional `tab`, `sort` on a first page | 1 credit/page |
| `GET /channel/playlists?channel=@mkbhd` (or `&continuation=`) | `channel` or `continuation` | 1 credit/page |
| `GET /channel/posts?channel=@mkbhd` (or `&continuation=`) | `channel` or `continuation` | 1 credit/page |
| `GET /channel/banner?channel=@mkbhd` | `channel` | 1 credit |
| `GET /video/related?v=<id or URL>` | `v` | 1 credit |
| `GET /video/dislikes?v=<id or URL>` | `v` | 1 credit |
| `GET /channel/search?channel=@mkbhd&q=iphone` (or `&continuation=`) | `channel`+`q`, or `continuation` | 1 credit/page |
| `GET /playlist?list=<id or URL>` (or `&continuation=`) | `list` or `continuation` | 1 credit/page |

See the `fetch-transcript` skill for how to obtain `YOUTUBE_TRANSCRIPT_API_KEY`, and for the shared error-code table (401/402/404/429/503).

## Which one to reach for

- Just want to know what's new? `get_channel_latest_videos` / `channel/latest` - it's one credit for the whole latest shelf, use it before reaching for the paginated full-history tool.
- Need the complete history (for an archive, a database, or "has this channel ever covered X")? `list_channel_videos` / `channel/videos`. Add `sort=popular` for the most-viewed first, or `tab=shorts` / `tab=streams` for Shorts or live streams.
- Only care about one topic within a channel? `search_channel_videos` / `channel/search` - cheaper than paginating the full history and filtering yourself.
- Before paginating a large channel or playlist, consider checking remaining balance first (`get_credits` tool, or `GET /credits`, both free) so a long pagination loop doesn't die partway through on a `402`.
