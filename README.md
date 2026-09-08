# AnimeSV

AnimeSV is a portable desktop application for tracking anime progress, fetching metadata, and finding releases across multiple providers. It combines a personal database with an integrated torrent client and search engine.

## Key Features

- **Multi-Provider Search**: Search **Nyaa.si**, **SubsPlease**, **HiAnime**, and **SeaDex** from one interface.
- **Integrated Downloader**: Built-in libtorrent client for downloading episodes directly, or open magnet links in an external client.
- **Smart Tracking**: Remembers your last watched episode, quality, and fansub group to find the next episode automatically.
- **SxxExx Parsing**: Extracts season/episode identifiers from filenames (supports `S01E05`, `S01 - 05`, `Ep05`, and more). Toggle the SE column from the hamburger menu.
- **Metadata Fetching**: Pulls cover art, descriptions, genres, and titles from MyAnimeList via Tenrai or Jikan APIs.
- **Seasonal Browser**: Browse current and past seasons with genre filtering, origin detection (Chinese/Japanese), and enrichment status.
- **Watch Folder Automation**: Trigger downloads by creating a file in your download directory.
- **Autodownloader**: Bulk scan your entire collection for new episodes.
- **Configurable Scrape Backend**: Choose BeautifulSoup (default) or Scrapling (adaptive, anti-bot bypass).
- **Requirements Check**: Detects missing Python modules at startup and via the hamburger menu.
- **Customizable UI**: Theme editor with color/font options, cross-platform scroll wheel support, ESC-to-close on all dialogs.

---

## Getting Started

### Standalone Binary (Recommended)

1. Download `anime_sv.bin` (Linux) or `anime_sv.exe` (Windows) from the releases page.
2. Make it executable (Linux):
   ```bash
   chmod +x anime_sv.bin
   ```
3. Run:
   ```bash
   ./anime_sv.bin
   ```

On first launch, the app creates its data directory at `~/.local/share/animescape/` (Linux) and initializes the database.

### Running from Source

```bash
git clone <repo-url>
cd animeScrape
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python anime_sv.py
```

---

## Requirements

### Python Packages

| Package | Purpose | Required |
|---|---|---|
| `requests` | HTTP client | Yes |
| `beautifulsoup4` | HTML parsing (default scraper) | Yes |
| `Pillow` | Cover art image processing | Yes |
| `scrapling[all]` | Adaptive web scraping with anti-bot bypass | Optional |
| `yt-dlp` | Video/stream extraction (used as CLI tool) | Optional |
| `libtorrent` | Internal torrent client | Optional |
| `Nuitka` | Building standalone binary | Build only |
| `platformdirs` | Cross-platform data directory resolution | Optional (fallback available) |

Install all at once:
```bash
pip install -r requirements.txt
pip install libtorrent  # separate due to binary dependency
```

### System Dependencies

**Japanese/CJK Font Support** (required for correct title rendering):

| Distro | Command |
|---|---|
| Debian/Ubuntu | `sudo apt install fonts-noto-cjk` |
| Arch Linux | `sudo pacman -S noto-fonts-cjk` |
| Fedora | `sudo dnf install google-noto-sans-cjk-fonts` |

### Checking Module Availability

The app checks for required modules at startup. You can also check manually via the hamburger menu: **Check Python Modules**. This shows which modules are installed in the current Python environment vs a `.venv`, and provides exact `pip install` commands for anything missing.

---

## How to Use

### Building Your Collection
1. Click the **Hamburger Menu (☰)** → **Manage Titles**.
2. Click **Add New Title**, type a name (e.g., *"Frieren"*), and select the match.
3. The app scrapes metadata and adds it to your library.

### Finding Releases
1. Select an anime using **<< Prev** / **Next >>** or the slider.
2. Choose a **Source** from the dropdown (default: *Nyaa*).
3. Results appear in the right pane.
4. **Right-click** a result → **Download (Internal)** or **Fetch & Watch (External)**.

### Tracking Progress
- Every download is recorded. The app remembers your preferred **Group**, **Quality**, and **Episode**.
- **Find Next Ep** searches for the next episode based on your history.

### Watch Folder Automation
1. Enable **Watch Folder** checkbox in the main window.
2. Create a file named `Search Term.animeSV` in your download directory (e.g., `Frieren 05.animeSV`).
3. AnimeSV detects it, finds the best match, and starts the download.
4. The filename updates to show progress: `Frieren 05.20%` → `Frieren 05.completed`.

---

## Supported Providers

| Provider | Type | Notes |
|---|---|---|
| **Nyaa** | Torrents | General anime torrents (default) |
| **SubsPlease** | Torrents | Direct releases from SubsPlease |
| **HiAnime** | Streaming | Web-based streaming source |
| **SeaDex** | Index | Best-release index (requires AniList ID) |
| **AllAnime** | ~~Streaming~~ | DEFUNCT (Cloudflare + API encryption) |

### API & Scrape Providers

- **API Provider** (hamburger menu): **Tenrai** (default, faster) or **Jikan** for MAL data.
- **Scrape Provider** (hamburger menu): **BeautifulSoup** (default) or **Scrapling** (adaptive, ~2x faster, anti-bot bypass).

---

## Configuration

Edit `options.txt` in the data directory (`~/.local/share/animescape/options.txt`):

| Key | Description | Default |
|---|---|---|
| `INTERNAL_TORRENT_DL_DIR` | Torrent download location | `~/.local/share/animescape/` |
| `OUTPUT_MAGNET_FILES_DIR` | Magnet file output directory | `~/.local/share/animescape/magnets/` |
| `UI_USE_CUSTOM_THEME` | Enable custom colors/fonts | `False` |
| `WATCH_FOLDER_ACTIVE` | Enable watch folder on startup | `False` |
| `API_PROVIDER` | MAL API backend | `tenrai` |
| `SCRAPE_PROVIDER` | Web scraping backend | `beautifulsoup` |
| `SHOW_SE_COLUMN` | Show Series/Episode column in search results | `False` |
| `TORRENT_PROFILE` | libtorrent performance profile | `min_memory` |

---

## Data Directory

All persistent data lives in `~/.local/share/animescape/` (Linux). The app migrates existing data from the working directory on first run.

```
~/.local/share/animescape/
├── options.txt              # User preferences
├── torrent_client.log       # Torrent client log
├── database/
│   ├── anime_details.csv    # Anime metadata
│   ├── search_terms.csv     # Saved search terms
│   ├── episode_history.csv  # Download history
│   ├── auto_downloads.csv   # Autodownload records
│   ├── anime_data.csv       # Query/MAL mapping
│   ├── seasonal_cache.json  # Seasonal browser cache
│   └── filter_presets.json  # Genre filter presets
├── images/                  # Cached cover art
│   └── seasonal/            # Seasonal browser thumbnails
├── backups/                 # Automatic database backups
└── fonts/                   # Custom fonts (optional)
```

Override with the `ANIMESCAPE_DATA_DIR` environment variable.

---

## Building

```bash
# Install build dependencies
pip install nuitka ordered-set
pip install -r requirements.txt
pip install libtorrent

# Build standalone binary
bash build_app.sh
```

CI builds for Linux and Windows run automatically via GitHub Actions on push.

---

## Seasonal Browser

- **Sort**: By Release Date, Title, or Origin (Chinese → Japanese → Unknown)
- **Filter**: Genre filter with presets (Save, Load, Rename, Delete)
- **Details Pane**: Info tab (synopsis, metadata) and Status tab (enrichment log)
- **Stale item pruning**: Shows older than 1 year removed automatically
- **Origin detection**: Detects Chinese/Japanese origin from producer/studio data
