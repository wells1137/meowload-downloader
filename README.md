# MeowLoad Downloader

**Download videos, images, and audio without watermarks from 999+ platforms — right inside Cursor.**

No API key needed. No configuration. Install the plugin and start downloading.

## What it does

Paste any social media link in your Cursor chat, and the agent will extract watermark-free media for you:

- Direct download URLs for videos, images, and audio
- Multi-resolution support (up to 8K for YouTube)
- Sora2 watermark removal (original quality, zero loss)
- Subtitle extraction (YouTube, multiple languages and formats)
- Batch extraction from playlists, channels, and profiles

## Supported Platforms (999+)

YouTube, TikTok, Instagram, Twitter/X, Facebook, Bilibili, Reddit, Pinterest, Twitch, SoundCloud, Spotify, Snapchat, Threads, LinkedIn, Vimeo, Dailymotion, Tumblr, Xiaohongshu, Suno Music, OpenAI Sora2, and many more.

## Usage

After installing the plugin, simply tell the agent what you want in chat:

```
Download this video: https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

```
Extract all videos from this channel: https://www.youtube.com/@Nike/videos
```

```
Remove the Sora watermark from this video: https://sora.chatgpt.com/p/s_xxxxx
```

```
Get subtitles for this YouTube video: https://www.youtube.com/watch?v=xxxxx
```

The agent will call the MeowLoad API, parse the results, and give you direct download links or save files locally.

## Features

| Feature | Description |
|---------|-------------|
| Single Post Download | Extract media from any single post URL |
| Batch Extraction | Download entire playlists, channels, or profiles with pagination |
| Multi-Resolution | Choose from multiple quality options (144p to 8K) |
| Sora2 Watermark Removal | Get original watermark-free Sora2 videos |
| Subtitle Download | Extract YouTube subtitles in SRT, VTT, TTML, and more |
| Credits Check | Monitor remaining API usage |

## How it works

This plugin wraps the [MeowLoad (哼哼猫)](https://www.henghengmao.com) developer API. An API key is built-in so you can use it immediately. If you prefer to use your own API key, set the `MEOWLOAD_API_KEY` environment variable.

## Credits

Powered by [MeowLoad API](https://docs.henghengmao.com/developer).

## License

MIT
