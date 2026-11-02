# yt-playlist-downloader

Downloads YouTube playlists as `.ogg` audio files and keeps them up to date. Run it again later: new tracks get downloaded, tracks you removed from the playlist get deleted.

## Install

You need `bash` 4+, `yt-dlp` and `ffmpeg`.

| System        | Command                                              |
|---------------|------------------------------------------------------|
| Arch, CachyOS | `sudo pacman -S yt-dlp ffmpeg`                       |
| Debian, Ubuntu| `sudo apt install ffmpeg pipx && pipx install yt-dlp`|
| Fedora        | `sudo dnf install yt-dlp ffmpeg`                     |
| macOS         | `brew install bash yt-dlp ffmpeg`                    |

Then copy `yt-playlist-downloader` into `~/.local/bin/` and make it executable:

```sh
chmod +x ~/.local/bin/yt-playlist-downloader
```

## Usage

Put your playlist URLs in `~/.config/yt-playlist-downloader/playlists.conf`, one per line:

```
https://www.youtube.com/playlist?list=PLxxxxxxxx
https://www.youtube.com/playlist?list=PLyyyyyyyy
```

Run it:

```sh
yt-playlist-downloader              # downloads to ~/Music
yt-playlist-downloader ~/MyMusic    # downloads to another folder
yt-playlist-downloader -u URL       # one playlist, without the config file
```

See `yt-playlist-downloader --help` for all options.

Each playlist gets its own folder. A file called `.archive` keeps track of what was downloaded. If you delete it, everything is downloaded again.

## Errors

**HTTP Error 403**: YouTube is rate limiting you, usually after many requests in a short time. The script waits and retries up to 3 times. If it still fails, wait a few minutes and run it again.

**"Sign in to confirm you're not a bot", or a private playlist**: use the login from a browser where you're signed into YouTube:

```sh
yt-playlist-downloader --cookies-from-browser firefox    # or chrome, chromium, brave, vivaldi
```

For other Firefox-based browsers (Waterfox, LibreWolf), give the path to the profile folder:

```sh
yt-playlist-downloader --cookies-from-browser "firefox:$HOME/.waterfox/xxxxxxxx.default-release"
```

To always use it, add `export YT_PLAYLIST_DOWNLOADER_BROWSER=firefox` to your `~/.bashrc` or `~/.zshrc`.

**Downloads suddenly stop working**: update yt-dlp.

## On your phone

The easiest way is to run this on your computer and copy the music folder to your phone with [Syncthing](https://syncthing.net/). Tracks you delete on the computer get deleted on the phone too.

## Limitations

- A song that's in two playlists only gets downloaded into the first one.
