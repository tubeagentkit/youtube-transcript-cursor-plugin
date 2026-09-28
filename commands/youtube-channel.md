---
name: youtube-channel
description: Look up a YouTube channel's recent or full upload history, search within a channel, or resolve a handle to a channel ID.
---

# /youtube-channel

Browse or search a YouTube channel.

## Usage

```
/youtube-channel <@handle, channel URL, or UC... ID> [--all | --search <query>]
```

- No flag: return channel metadata plus its recent uploads (free).
- `--all`: paginate the channel's complete upload history.
- `--search <query>`: search within the channel's videos for a query.

## Behavior

Follow the `channel-playlist-research` skill: prefer the bundled `youtube-transcript` MCP server's `get_channel_latest_videos`, `list_channel_videos`, or `search_channel_videos` tools (matching the flag used); fall back to the corresponding REST endpoints (`/channel/latest`, `/channel/videos`, `/channel/search`) if the MCP server isn't connected. For `--all` or `--search`, warn the user before paginating a very large channel, since each page after the first costs a credit.

## Example

```
/youtube-channel @veritasium
/youtube-channel @mkbhd --search iphone
/youtube-channel @TED --all
```
