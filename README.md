# AnimeSV (Standalone)

AnimeSV is a portable, all-in-one desktop application for tracking your anime progress, fetching metadata, and finding releases on Nyaa.si. 

This standalone version (`anime_sv.bin`) comes with everything included—no Python installation or complex setup required.

## ✨ Key Features

*   **Portable**: Runs as a single executable file. Keep it on a USB drive or anywhere on your system.
*   **Integrated Downloader**: Includes a built-in torrent client (libtorrent) to download episodes directly within the app.
*   **Smart Tracking**: Automatically remembers your last watched episode, quality, and fansub group to help you find the next episode instantly.
*   **Metadata Fetching**: Pulls cover art, descriptions, and titles from MyAnimeList/Jikan.
*   **Autodownloader**: Can automatically scan for and download new episodes for your entire list.

---

## 🚀 Getting Started

### 1. Installation
Since this is a standalone binary, there is no "installation" process.
1.  Download `anime_sv.bin` from the releases page.
2.  Open your terminal and navigate to the download location.
3.  Make the file executable:
    ```bash
    chmod +x anime_sv_x86_64
    ```

### 2. First Run
Run the application:
```bash
./anime_sv_x86_64
```

**On the first launch**, the app will detect that it is running in a new location. It will ask to initialize its file structure. Click **Yes**.

This will create the following items in the same directory as the binary:
*   `database/`: Stores your anime list and history (CSV files).
*   `images/`: Stores cached cover art.
*   `backups/`: Stores automatic backups of your data.
*   `options.txt`: A configuration file for customizing paths and colors.

---

## 📖 How to Use

### Step 1: Build Your Collection
When you first open the app, your list will be empty.
1.  Click the **Hamburger Menu (☰)** in the top-right corner of the details pane.
2.  Select **Manage Anime Shows**.
3.  Click **Add New Title**.
4.  Type the name of an anime (e.g., *"Frieren"*) and press Enter.
5.  Select the correct match from the list. The app will scrape the metadata and add it to your database.
6.  Repeat for as many shows as you like, then close the Manage window.

### Step 2: Finding Releases
Back in the main window:
1.  Use the **<< Prev** and **Next >>** buttons (or the slider) to select an anime.
2.  The right-hand pane will automatically search Nyaa.si for torrents matching that anime.
3.  **To Download**:
    *   **Right-click** a result in the list.
    *   Select **Download (Internal)** to start downloading immediately in the bottom pane.
    *   *Alternatively*, select **Fetch & Watch** to open the magnet link in your system's default torrent client (e.g., qBittorrent).

### Step 3: Tracking Progress
Once you download an episode via the app, it is recorded in your **History**.
*   The app remembers the **Fansub Group** (e.g., `[SubsPlease]`) and the **Quality** (e.g., `1080p`).
*   Next time you select that anime, you can click the **Find Next Ep** button. The app will intelligently search for the *next* episode number using your preferred group and quality.

---

## ⚙️ Configuration

You can customize the application by editing the `options.txt` file generated after the first run.

*   **`INTERNAL_TORRENT_DL_DIR`**: Change where the built-in downloader saves video files.
*   **`UI_USE_CUSTOM_THEME`**: Set to "True" to enable custom background colors and fonts.
*   **`TORRENT_PROFILE`**: Switch between "min_memory" (lightweight) and "high_performance" (faster speeds).
