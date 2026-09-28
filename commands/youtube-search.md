---
name: youtube-search
description: Search YouTube for videos or channels matching a query.
---

# /youtube-search

Search YouTube for videos, or search for a channel by name/handle.

## Usage

```
/youtube-search <query> [--type video|channel]
```

`--type` defaults to `video`. Use `channel` to search for channels instead of videos.

## Behavior

Follow the `search-youtube` skill: prefer the bundled `youtube-transcript` MCP server's `search_youtube` tool; fall back to the REST API (`GET /api/v1/search`) if the MCP server isn't connected. Present results as a short list (title, channel, and URL/ID) rather than raw JSON. Offer to paginate with the response's continuation token if the user wants more results.

## Example

```
/youtube-search machine learning explainers
/youtube-search mkbhd --type channel
```
