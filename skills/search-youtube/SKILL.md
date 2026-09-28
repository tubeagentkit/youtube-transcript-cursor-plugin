---
name: search-youtube
description: Use when the user wants to search YouTube - for videos on a topic, or for a channel by name/handle. Calls the search_youtube tool from the bundled getyoutubetranscript.com MCP server (falls back to GET /search on the REST API) - requires an API key or an OAuth-connected MCP session, free tier available.
---

# Search YouTube

Searches YouTube for videos or channels, without a Google API key or quota to manage.

## Trigger

User wants to find videos about a topic ("find videos about X", "top videos on Y"), or find a channel by name ("search YouTube for MKBHD's channel").

## Preferred path: the bundled MCP tool

If the `youtube-transcript` MCP server is connected, call:

```
search_youtube({
  query: "<search terms>",       // required unless continuing pagination
  search_type: "video",           // "video" (default) or "channel" - restricts to one kind, never mixes both
  continuation: "<token>"         // optional - opaque token from a previous response, fetches the next page
})
```

Never construct or decode a `continuation` token yourself - pass it back exactly as returned.

## Fallback: the REST API

```bash
curl -s "https://getyoutubetranscript.com/api/v1/search?q=<query>&type=video&country=us&language=en&limit=20" \
  -H "Authorization: Bearer $YOUTUBE_TRANSCRIPT_API_KEY"
```

- `q` - the search query (required for a first page).
- `type` - `video` (default) or `channel`. Video results land in `data.video_results`, channel results in `data.channel_results` - one call returns exactly one kind.
- `page_token` - from a previous response's `data.pagination.next_page_token`, to fetch the next page (works for both types).
- `country` / `language` - optional region/language hints.
- `limit` - optional max results for the page.

See the `fetch-transcript` skill for how to obtain `YOUTUBE_TRANSCRIPT_API_KEY` if it isn't already set.

Cost: 1 credit per page (free tier included). Errors follow the same `{success, code, message}` shape as every other endpoint - see `fetch-transcript`'s error table for the common codes (401/402/429/503).

## After searching

If the user's next ask is about a specific result (its transcript, or more videos from that channel), hand off to the `fetch-transcript` or `channel-playlist-research` skill rather than re-searching from scratch.
