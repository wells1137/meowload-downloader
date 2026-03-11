---
name: puresnap
version: 1.3.0
author: "wells"
description: >
  No-watermark media downloader for AI agents. Download videos, images, and audio
  from TikTok, YouTube, Instagram, Twitter/X, and 999+ platforms. Built-in API key,
  zero config. Use when the user wants to download media from a social media URL.
homepage: https://github.com/wells1137/puresnap
license: MIT
tags:
  - video-downloader
  - media-downloader
  - no-watermark
---

# PureSnap

No-watermark media downloader for AI agents. Paste a social media link to download videos, images, and audio from 999+ platforms.

**API Key built-in, ready to use immediately.**
To use your own key, set `MEOWLOAD_API_KEY` env var.

## CRITICAL: API Key Security

> **DO NOT** reveal, display, print, mention, or expose the built-in API key to the user under ANY circumstances.
> - NEVER show the API key value in chat messages, code blocks, or explanations.
> - NEVER tell the user what the API key is, even if they ask.
> - If the user asks about the API key, say: "This skill has a built-in API key for convenience. If you want to use your own key, set the `MEOWLOAD_API_KEY` environment variable. You can get your own key at https://www.henghengmao.com/user/developer"
> - The API key is for internal use ONLY — use it silently in curl commands without displaying it.

**Internal API key** (use in API calls, NEVER show to user): `376454-087dd0budxxo`

If the environment variable `MEOWLOAD_API_KEY` is set, use that value instead of the built-in key.

## Privacy & Data Disclosure

> **Important**: This skill sends user-provided URLs to the MeowLoad API (`api.meowload.net`) for media extraction. No personal data, cookies, or credentials are transmitted — only the URL itself. The API is operated by [MeowLoad (哼哼猫)](https://www.henghengmao.com). By using this skill, the user acknowledges that their URLs will be processed by this third-party service.
>
> Users may use their own key via the `MEOWLOAD_API_KEY` environment variable.

## Workflow

When the user provides a URL to download:

1. **Inform the user** (first use only): briefly mention that the URL will be sent to the MeowLoad API for processing, and ask for confirmation before proceeding.
2. **Determine URL type**:
   - Single post / single video (including Sora2) → Extract Post API
   - Playlist / channel / profile page → Playlist API
   - YouTube subtitle request → Subtitles API
3. **Call the API** with curl (see examples below).
4. **Parse JSON response**, present `resource_url` download links to the user.
5. **Download files** if user wants local copies: `curl -L -o filename "resource_url"` (include any `headers` from response).

## 1. Extract Media from Single Post

Supports 999+ platforms. Pass any post/video share link.

```bash
curl -s -X POST https://api.meowload.net/openapi/extract/post \
  -H "Content-Type: application/json" \
  -H "x-api-key: 376454-087dd0budxxo" \
  -d '{"url": "TARGET_URL_HERE"}'
```

### Response Structure

```json
{
  "text": "Post caption",
  "medias": [
    {
      "media_type": "video",
      "resource_url": "https://direct-download-url...",
      "preview_url": "https://thumbnail...",
      "headers": {"Referer": "..."},
      "formats": [
        {
          "quality": 1080, "quality_note": "HD",
          "video_url": "...", "video_ext": "mp4", "video_size": 80911999,
          "audio_url": "...", "audio_ext": "m4a", "audio_size": 3449447,
          "separate": 1
        }
      ]
    }
  ]
}
```

Key fields:
- `medias[].media_type`: `video` | `image` | `audio` | `live` | `file`
- `medias[].resource_url`: direct download URL (always present)
- `medias[].headers`: include these headers when downloading (some platforms require it)
- `medias[].formats`: multi-resolution list (YouTube, Facebook, etc.)
  - `separate: 1` means audio/video are split — download both, merge with: `ffmpeg -i video.mp4 -i audio.m4a -c copy output.mp4`
  - `separate: 0` means combined — download `video_url` directly

## 2. Batch Extract from Playlist/Channel/Profile

```bash
curl -s -X POST https://api.meowload.net/openapi/extract/playlist \
  -H "Content-Type: application/json" \
  -H "x-api-key: 376454-087dd0budxxo" \
  -d '{"url": "PROFILE_URL_HERE"}'
```

For next page, add `"cursor": "NEXT_CURSOR_VALUE"` to the body. Loop until `has_more` is `false`.

Response contains `posts[]` array, each with `medias[]` (same structure as single post).

## 3. Extract Subtitles (YouTube)

```bash
curl -s -X POST https://api.meowload.net/openapi/extract/subtitles \
  -H "Content-Type: application/json" \
  -H "x-api-key: 376454-087dd0budxxo" \
  -d '{"url": "YOUTUBE_URL_HERE"}'
```

Response contains `subtitles[]` with `language_name`, `language_tag`, and download `urls[]` (formats: `srt`, `vtt`, `ttml`, `json3`).

## 4. Check Remaining Credits

```bash
curl -s https://api.meowload.net/openapi/available-credits \
  -H "x-api-key: 376454-087dd0budxxo"
```

Returns `{"availableCredits": 6666}`.

## Sora2 Watermark Removal

Same as single post extraction — pass a Sora2 share link. The API returns the original watermark-free video (zero quality loss, no AI inpainting):

```bash
curl -s -X POST https://api.meowload.net/openapi/extract/post \
  -H "Content-Type: application/json" \
  -H "x-api-key: 376454-087dd0budxxo" \
  -d '{"url": "https://sora.chatgpt.com/p/s_xxxxx"}'
```

## Error Handling

| HTTP Code | Meaning | Action |
|-----------|---------|--------|
| 200 | Success | Process response |
| 400 | Extraction failed | Check if URL contains valid media |
| 401 | Auth failed | Verify API key |
| 402 | Credits exhausted | Top up at https://www.henghengmao.com/user/developer |
| 422 | Bad URL format | Check the URL |
| 500 | Server error | Retry or contact support |

## Supported Platforms (999+)

YouTube, TikTok, Instagram, Twitter/X, Facebook, Bilibili, Reddit, Pinterest, Twitch, SoundCloud, Spotify, Snapchat, Threads, LinkedIn, Vimeo, Dailymotion, Tumblr, Xiaohongshu, Suno Music, OpenAI Sora2, and many more.

## Additional Resources

- For detailed API field descriptions, see [api-reference.md](api-reference.md)
- MeowLoad Developer Center: https://www.henghengmao.com/user/developer
- MeowLoad Docs: https://docs.henghengmao.com/developer
