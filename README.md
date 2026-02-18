# yt2mp3 🎵

A powerful, concurrent YouTube to MP3 downloader with playlist support, ID3 metadata embedding, SponsorBlock integration, and a beautiful live progress UI.

---

## Features

- **Concurrent downloads** — download multiple tracks simultaneously with configurable worker threads
- **Playlist support** — auto-detects YouTube playlists and downloads every track
- **Quality presets** — from `voice` (64kbps) to `best` (320kbps), with custom bitrate override
- **ID3 metadata** — auto-embeds title, artist, album, and year into every MP3
- **Album art** — optionally embeds the video thumbnail directly into the MP3
- **SponsorBlock** — strips sponsor segments, intros, outros, and self-promos from audio
- **Audio normalization** — loudness-normalizes output via FFmpeg
- **Cookies support** — access age-restricted or members-only content via a cookies file
- **Bulk input** — pass a `.txt` file with one URL per line
- **Rich UI** — live progress bars with speed, ETA, and file size per track
- **Info mode** — inspect track metadata without downloading anything
- **Auto-installs** — installs Python dependencies automatically on first run

---

## Requirements

### System dependencies

| Dependency | Install |
|---|---|
| Python 3.10+ | [python.org](https://python.org) |
| ffmpeg | See below |

**Install ffmpeg:**

```bash
# macOS
brew install ffmpeg

# Ubuntu / Debian
sudo apt install ffmpeg

# Windows (via Chocolatey)
choco install ffmpeg

# Windows (via Winget)
winget install ffmpeg
```

### Python dependencies (auto-installed)

- `yt-dlp`
- `rich`

---

## Installation

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/yt2mp3.git
cd yt2mp3

# 2. (Optional) Create a virtual environment
python -m venv venv
source venv/bin/activate      # macOS / Linux
venv\Scripts\activate         # Windows

# 3. Install Python dependencies
pip install yt-dlp rich

# 4. Run it
python yt2mp3.py --help
```

---

## Usage

### Basic download

```bash
python yt2mp3.py https://youtu.be/dQw4w9WgXcQ
```

### High quality with metadata and thumbnail

```bash
python yt2mp3.py https://youtu.be/dQw4w9WgXcQ -q best --thumbnail --metadata
```

### Download a full playlist with 4 concurrent workers

```bash
python yt2mp3.py "https://youtube.com/playlist?list=PLxxxxxx" --workers 4
```

### Download from a text file of URLs

```bash
python yt2mp3.py urls.txt -o ~/Music -q high
```

### Strip sponsors + normalize audio

```bash
python yt2mp3.py https://youtu.be/dQw4w9WgXcQ --sponsorblock --normalize
```

### Inspect track info without downloading

```bash
python yt2mp3.py https://youtu.be/dQw4w9WgXcQ --info
```

### Age-restricted content via cookies

```bash
python yt2mp3.py https://youtu.be/dQw4w9WgXcQ --cookies cookies.txt
```

---

## CLI Reference

```
usage: yt2mp3 [-h] [-o OUTPUT] [-q {best,high,medium,low,voice}]
              [--bitrate BITRATE] [--thumbnail] [--metadata] [--no-metadata]
              [--sponsorblock] [--normalize] [--workers WORKERS]
              [--filename FILENAME] [--cookies COOKIES]
              [--flat-playlist] [--info]
              urls [urls ...]
```

| Flag | Default | Description |
|---|---|---|
| `urls` | — | YouTube URL(s), or a `.txt` file with one URL per line |
| `-o`, `--output` | `./downloads` | Output directory |
| `-q`, `--quality` | `high` | Quality preset: `best`, `high`, `medium`, `low`, `voice` |
| `--bitrate` | — | Override bitrate in kbps (e.g. `320`) |
| `--thumbnail` | off | Embed album art thumbnail into the MP3 |
| `--metadata` | on | Embed ID3 metadata (title, artist, year, etc.) |
| `--no-metadata` | — | Disable metadata embedding |
| `--sponsorblock` | off | Remove sponsor segments via SponsorBlock |
| `--normalize` | off | Normalize audio loudness via FFmpeg |
| `--workers` | `3` | Number of concurrent downloads |
| `--filename` | `%(uploader)s - %(title)s.%(ext)s` | Output filename template |
| `--cookies` | — | Path to a `cookies.txt` for age-restricted content |
| `--flat-playlist` | on | Resolve playlist URLs to individual track URLs |
| `--info` | off | Print track metadata only, skip download |

---

## Quality Presets

| Preset | Bitrate | Sample Rate | Best For |
|---|---|---|---|
| `best` | 320 kbps | 48,000 Hz | Archival, audiophiles |
| `high` | 256 kbps | 44,100 Hz | Music listening |
| `medium` | 192 kbps | 44,100 Hz | General use |
| `low` | 128 kbps | 44,100 Hz | Storage-conscious |
| `voice` | 64 kbps | 22,050 Hz | Podcasts, spoken word |

---

## Filename Templates

The `--filename` flag accepts [yt-dlp output template syntax](https://github.com/yt-dlp/yt-dlp#output-template). Some useful variables:

| Variable | Example Output |
|---|---|
| `%(title)s` | Never Gonna Give You Up |
| `%(uploader)s` | Rick Astley |
| `%(upload_date)s` | 20091025 |
| `%(id)s` | dQw4w9WgXcQ |
| `%(duration)s` | 212 |

**Example:**
```bash
python yt2mp3.py https://youtu.be/xyz --filename "%(upload_date)s - %(title)s.%(ext)s"
```

---

## Cookies (Age-Restricted Content)

To download age-restricted or members-only content, export your browser cookies and pass the file:

1. Install the [Get cookies.txt LOCALLY](https://chrome.google.com/webstore/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc) Chrome extension (or equivalent for Firefox)
2. Visit YouTube while logged in
3. Export cookies as `cookies.txt`
4. Run:

```bash
python yt2mp3.py https://youtu.be/xyz --cookies cookies.txt
```

---

## Bulk Downloads via Text File

Create a `urls.txt` file with one URL per line:

```
https://youtu.be/dQw4w9WgXcQ
https://youtu.be/9bZkp7q19f0
https://youtu.be/kJQP7kiw5Fk
```

Then run:

```bash
python yt2mp3.py urls.txt --workers 5 -q best -o ~/Music
```

---

## Output

After a run completes, a summary table is printed:

```
╭──────────────────────────────────────────────────────────╮
│ #  │ Title                  │ Status │  Size  │ Error    │
├────┼────────────────────────┼────────┼────────┼──────────┤
│ 1  │ Rick Astley - Never…   │  ✓ OK  │ 7.2 MB │          │
│ 2  │ PSY - GANGNAM STYLE    │  ✓ OK  │ 9.1 MB │          │
│ 3  │ Restricted Video       │ ✗ FAIL │   —    │ 403 ...  │
╰──────────────────────────────────────────────────────────╯
  Succeeded: 2   Failed: 1   Total: 3
```

---

## Troubleshooting

**`ffmpeg not found`**
Install ffmpeg for your platform (see [Requirements](#requirements)).

**`ERROR: Sign in to confirm your age`**
Use `--cookies cookies.txt` with a valid cookies file from a logged-in YouTube session.

**`HTTP Error 429: Too Many Requests`**
Reduce `--workers` to `1` or `2`. YouTube may throttle aggressive concurrent requests.

**Download is very slow**
Try increasing `--workers`. Also ensure you're on a fast connection — yt-dlp uses concurrent fragment downloads internally.

**SponsorBlock not working**
SponsorBlock data only exists for videos that have been submitted to the community database at [sponsor.ajay.app](https://sponsor.ajay.app).

---

## Legal Notice

This tool is intended for downloading content you have the right to download (e.g. Creative Commons music, your own uploads, or content explicitly allowed by the creator). Downloading copyrighted material without permission may violate YouTube's Terms of Service and applicable copyright law. Use responsibly.

---


