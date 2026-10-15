# Klingklang

A music player that plays straight from a folder on your computer. There is no
library and no import: every time you open a folder, the app reads it fresh, so
syncing new music into the folder is all you need to do to update it.

Everything runs in the browser. Your music never leaves your computer, and the
app works without an internet connection once it has been opened once.

## Using it

1. Open the app in **Microsoft Edge** or **Google Chrome** and choose your music
   folder. (Firefox and Safari can't open folders this way.)
2. Install it from the browser menu (Edge: *Apps → Install this site as an app*).
3. Next time, the app opens straight to your music. If the browser asks for
   permission again, choose **Allow on every visit** and it won't ask anymore.

Browse folders, click a song to play it and everything after it in that folder,
or use **Play all** / **Shuffle**. Click the song in the player bar to jump to
its folder. The app remembers the song and position you stopped at.

The list button next to ⏭ opens **Up next**: every song in playing order (shuffled order after Shuffle), with
the current one highlighted. Tap a song to jump to it.

## Several music folders

Keep things like music and audiobooks apart by adding each as its own folder.

- **Home** shows all your music folders as big tiles, plus one to add another.
  The app starts here.
- **The path bar** at the top reads *Home › music folder › subfolders*; every
  part takes you there. Deeper than 3 subfolders, the ones in between fold
  into a ← button (`MAX_SUBFOLDERS` in `index.html`). On phones it shows
  Home, ← (one folder up) and where you are.
- **Settings (gear) → Manage music folders** removes folders from the list (no files are
  touched) and switches audiobook mode on or off.

Each folder remembers its own song and position, so switching to classical
music doesn't lose your place in an audiobook. If the browser asks for
permission again, tapping the folder's tile is all it takes.

## Audiobooks

Tick **Audiobooks** under a folder in *Settings (gear) → Manage music folders* (folders named with
"Hörbuch", "Hörspiel" or "Audiobook" are ticked automatically when added). In
such a folder every folder with songs in it counts as a book, and each book
remembers its own chapter and position:

- A started book shows **Continue** ("You stopped at …") and **Start over**,
  and gets a small *Started* label on its tile. There is no Shuffle.
- Continuing starts 5 seconds earlier (`RESUME_REWIND` in `index.html`).
- Finishing the last chapter clears the place, so the book starts from the
  beginning next time.

A book split into subfolders (`CD 1`, `CD 2`) counts as one book per subfolder.

### Narrator

In audiobook folders the book's page shows "Gesprochen von …" under the title.
It comes from the **Composer** field (where Audible download tools put the
narrator), or from **Conductor** when Composer is empty. Lists like
`A;B` are shown as "A, B". When the songs in a folder have different narrators
(podcasts, collections), each song shows its own. The player shows the first
name only, plus "& weitere" when there are more. Music folders never show it,
because there Composer means the composer.

### Chapters

M4B (and M4A) files with chapter marks are shown chapter by chapter: the book's
page lists the chapters with their length, ⏮ / ⏭ jump between chapters, the
player shows the chapter title with book and author below, and the progress
bar covers the current chapter. Both kinds of chapter marks are read (Apple's
chapter track and Nero's `chpl` list). This works in every folder, not only
audiobook folders.

## What it reads

- **Music:** only formats the browser plays by itself: MP3, M4A/AAC, FLAC,
  OGG, Opus, WAV. Other files are ignored.
- **Song details:** title, artist, album, track and disc number from the tags
  (ID3, Vorbis comments, MP4). Songs in a folder are sorted by disc and track
  number, then by file name. Details are kept in a small cache in the browser and
  re-read automatically when a file changes. *Settings (gear) → Forget saved song details*
  clears it.
- **Pictures:** `cover.jpg` in each folder, or else `cover.png`, `cover.webp`, `folder.jpg`,
  `folder.png`, `folder.webp` or `front.jpg` (see `COVER_NAMES` in `index.html`). It shows on
  the folder's tile, next to its title, and in the player while one of its
  songs plays. A folder without its own picture borrows its subfolders'
  pictures (up to `COVER_DEPTH` levels down, 2 by default), so an artist shows a 2×2 collage of their
  album covers, or the one album cover if there are fewer than four.
  Embedded pictures in the music files are not used.
- **Playlists:** `.m3u` and `.m3u8` files show up in the folder they're in. Paths
  inside are relative to the playlist file; both `/` and `\` work. Songs that
  can't be found are shown greyed out and skipped.

## Hosting

Live at <https://pheonton.github.io/klingklang/>.

Served as-is by GitHub Pages, no build step. The folder picker only works over
`https://` or on `localhost`, not from a `file://` page.

When you change any file, bump `VERSION` in `sw.js` so installed copies update.

## Customizing

- Texts are in `STRINGS` at the top of the script in `index.html` (English and
  German; the browser's language picks one). Add a language by copying a block.
- Colors are the variables at the top of the CSS, with a dark set under
  `prefers-color-scheme: dark`.

## Licenses

Atkinson Hyperlegible font: SIL Open Font License (`licenses/`). Icon shapes
follow Material Icons (Apache 2.0).
