---
name: youtube-transcript
description: Fetch the transcript of a YouTube video by URL or video ID, plus its title/author/thumbnail metadata.
---

# /youtube-transcript

Fetch the transcript for a YouTube video.

## Usage

```
/youtube-transcript <video URL or video ID> [language code]
```

Provide a YouTube URL (`youtube.com/watch?v=...`, `youtu.be/...`, `/shorts/...`) or a bare 11-character video ID. Language defaults to `en` if omitted.

## Behavior

Follow the `fetch-transcript` skill: prefer the bundled `youtube-transcript` MCP server's `get_youtube_transcript` tool; fall back to the REST API (`GET /api/v1/transcript`) if the MCP server isn't connected. Return the transcript as plain text along with the video's title and author. Note if the video has no transcript in the requested language, and never fabricate per-line timestamps - this API doesn't provide them.

## Example

```
/youtube-transcript https://www.youtube.com/watch?v=jNQXAC9IVRw
```
