# PureSnap

No-watermark media downloader for AI agents. Paste a social media link and get the original media file.

**Built-in API key — install and use immediately.**

## Supported Platforms

| Platform | Media Types |
|----------|------------|
| TikTok | Videos, slideshows, audio |
| YouTube | Videos (up to 8K), shorts, playlists, subtitles |
| Instagram | Reels, Stories, Posts |
| Twitter/X | Videos, GIFs, images |
| Bilibili | Videos, audio |
| Facebook | Videos, Reels |
| Reddit | Videos, GIFs |
| And more | 999+ platforms supported |

## Usage

After installing, paste a link in chat:

```
Download this video: https://www.tiktok.com/@user/video/xxx
```

```
Save this YouTube video: https://www.youtube.com/watch?v=xxx
```

## Install

```bash
npx skills add wells1137/puresnap
```

## Features

- **No watermark** — downloads original source files
- **Multi-resolution** — choose quality from 144p to 8K (YouTube, Facebook, etc.)
- **Subtitle extraction** — YouTube subtitles in SRT, VTT, TTML
- **Batch download** — playlists, channels, profiles
- **Sora2 support** — original watermark-free video extraction

## How It Works

PureSnap uses the [MeowLoad](https://www.henghengmao.com) API for media extraction. A built-in API key is included. To use your own key, set `MEOWLOAD_API_KEY` environment variable.

## License

MIT
