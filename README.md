# 🎵 Spotify Web Player Clone

A responsive, dynamic, client-side web player clone of Spotify built with HTML5, CSS3, and Vanilla JavaScript. This application mimics Spotify's music player interface and plays localized track listings dynamically using JSON configuration files.

---

## 🚀 Features

- **Dynamic Album & Playlist Loading**: Automatically reads available albums from a central configuration file, then pulls metadata (cover art, titles, and descriptions) and track listings on the fly.
- **Audio Control System**:
  - Play, pause, skip forward (next), and skip backward (previous).
  - Continuous timeline seekbar allowing users to scrub to any timestamp.
  - Precise volume control range slider with a toggle to mute and unmute audio.
- **Real-Time Visual State Updates**: Progress circle dynamically transitions across the seekbar reflecting current play-time relative to total duration.
- **Responsive Layout & Mobile Support**: Responsive sidebar with hamburger menu transition for a smooth mobile web player experience.
- **Minimalist & High-Performance Stack**: Zero heavy external JS frameworks, making loading times fast and client-side processing lightweight.

---

## 📂 Project Structure

```text
spotify/
├── css/
│   ├── style.css       # Core styles, layouts, positioning, and responsive design
│   └── utility.css     # Common utility classes (flex, align-center, margins, padding, custom scrollbar)
├── img/                # UI icons (play, pause, volume, mute, search, library, logo, etc.)
├── js/
│   └── script.js       # Core application logic (dynamic fetches, audio events, playbar controls)
├── songs/              # Audio directory and configurations
│   ├── albums.json     # List of folders corresponding to albums/playlists
│   ├── [album_folder]/ # Dynamic folder for each album
│   │   ├── cover.jpg   # Album cover artwork
│   │   ├── info.json   # Album metadata (title, description)
│   │   ├── songs.json  # Ordered list of audio filenames in this album
│   │   └── *.mp3       # MP3 audio tracks
│   └── ...
├── favicon.ico         # Spotify icon
└── index.html          # Main HTML5 layout and page structure
```

---

## 🛠️ Tech Stack

- **Markup**: [HTML5](https://developer.mozilla.org/en-US/docs/Web/HTML) for semantic layouts.
- **Styling**: [CSS3](https://developer.mozilla.org/en-US/docs/Web/CSS) (Vanilla) featuring Flexbox, transitions, custom scrollbar designs, and responsive layouts.
- **Scripting**: [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) (Vanilla ES6) utilizing standard Web Audio APIs, asynchronous `fetch` APIs, and DOM event listeners.
- **Database**: Localized JSON arrays/objects acting as a lightweight, serverless metadata database.

---

## ⚙️ How It Works

### 1. Album Initialization
On page load, the player calls `displayAlbums()` inside `script.js`, which:
1. Fetches `/songs/albums.json` to obtain the directory names of all registered albums.
2. Iterates through each album directory, fetching `/songs/[album_folder]/info.json` for custom display details.
3. Dynamically injects HTML cards displaying the title, description, and cover image (`/songs/[album_folder]/cover.jpg`) into the album grid.

### 2. Loading Tracks
When a user clicks on an album card:
1. The event listener triggers `getSongs('songs/[album_folder]')`.
2. It fetches `/songs/[album_folder]/songs.json` to load the list of available audio tracks.
3. It builds the tracklist inside the library panel (left sidebar) with clickable list items.
4. Auto-plays the first song in that album list using the browser's HTML5 `Audio()` object.

### 3. Media Controls & Timeline
- **Playback Control**: The play/pause buttons toggle `currentSong.play()` and `currentSong.pause()` while swapping the icon in the playbar.
- **Seeking**: The progress bar updates through the `timeupdate` event handler of the audio element. Clicking on the seekbar uses client offset width calculations to instantly modify `currentSong.currentTime`.
- **Volume and Mute**: Regulated by setting `currentSong.volume` based on range input coordinates. Toggle-mute keeps track of the previous volume state and updates the speaker graphic accordingly.

---

## 🚦 Getting Started

### Prerequisites
Because this project utilizes standard browser `fetch` requests to read JSON files, browsers block requests from direct `file://` protocol URLs due to CORS policies. **You must run this project on a local server.**

### Run Locally

#### Option 1: VS Code Live Server (Recommended)
1. Open the project folder in **Visual Studio Code**.
2. Install the **Live Server** extension (by Ritwick Dey) if you haven't already.
3. Click the **Go Live** button at the bottom-right corner of the status bar.
4. The application will open automatically at `http://127.0.0.1:5500/index.html`.

#### Option 2: Python HTTP Server
If you have Python installed, navigate to the project directory in your terminal/command prompt and run:
```bash
python -m http.server 8000
```
Then, open your browser and navigate to `http://localhost:8000`.

---

## 🎵 Adding Your Own Albums & Songs

To add a new album to the player:

1. **Create the Folder**: Under the `songs/` folder, create a new subdirectory (e.g., `synthwave`).
2. **Add Tracks**: Place your desired `.mp3` tracks into that subdirectory.
3. **Register Songs**: Create a `songs.json` file inside your new subdirectory. Add the exact filenames in an array:
   ```json
   [
     "Synth-Wave-Track-1.mp3",
     "Outrun-Melody-2.mp3"
   ]
   ```
4. **Add Album Info**: Create an `info.json` file inside the new subdirectory to set the cards text:
   ```json
   {
     "title": "Retro Synthwave",
     "description": "80s inspired retro beats for programming."
   }
   ```
5. **Set Album Cover**: Place a square image file named `cover.jpg` inside the folder to act as the cover art.
6. **Register the Album**: Open `songs/albums.json` at the root of the songs folder, and add your folder name to the array:
   ```json
   [
     "90s",
     "globalhiphop",
     "synthwave"
   ]
   ```
7. Refresh the page on your local server to load your new tracks!
