<p align='center'>
    <img alt='Logo' src='https://raw.githubusercontent.com/Marekkon5/onetagger/master/assets/onetagger-logo-github.png'>
</p>
<h1 align='center'>The ultimate cross-platform tagger for DJs</h1>

<h3 align='center'><b>
<a href='https://onetagger.github.io/'>Website</a> | <a href='https://github.com/Marekkon5/onetagger/releases/'>Latest Release</a>
</b></h3>
<br>

<p align='center'>
    <img alt='Version Badge' src='https://img.shields.io/github/v/release/marekkon5/onetagger?label=Latest%20Release'>
    <img alt='Supported OS' src='https://img.shields.io/badge/OS-Windows%2C%20Mac%20OS%2C%20Linux-orange'>
    <img alt='Build Status' src='https://img.shields.io/github/actions/workflow/status/marekkon5/onetagger/build.yml?branch=master'>
</p>

<h3 align='center'><b></b></h3>
<hr>

Cross-platform music tagger.
It can fetch metadata from Beatport, Traxsource, Juno Download, Discogs, Musicbrainz and Spotify.
It is also able to fetch Spotify's Audio Features based on ISRC & exact match. 
There is a manual tag editor and quick tag editor which lets you use keyboard shortcuts. Written in Rust, Vue.js and Quasar.

MP3, AIFF, FLAC, M4A (AAC, ALAC) supported.

*For more info and tutorials check out our [website](https://onetagger.github.io/).*

https://user-images.githubusercontent.com/15169286/193469224-cbf3af71-f6d7-4ecd-bdbf-5a1dca2d99c8.mp4


## Installing

You can download latest binaries from [releases](https://github.com/Marekkon5/onetagger/releases)

### Docker (CLI)

A CLI-only image can be published to GHCR as `ghcr.io/<owner>/onetagger-cli`.
On the main repository this resolves to `ghcr.io/marekkon5/onetagger-cli:latest`.
When testing from a fork, replace `marekkon5` with your GitHub namespace or override `ONETAGGER_IMAGE`.

Export the `config.json` shown by the GUI CLI dialog, then run the container with the same command you would use locally:

```sh
docker run --rm \
  -e PUID="$(id -u)" \
  -e PGID="$(id -g)" \
  -v /path/to/config.json:/config/input/config.json:ro \
  -v /path/to/music:/music \
  -v /path/to/onetagger-state:/config/onetagger \
  ghcr.io/marekkon5/onetagger-cli:latest \
  autotagger --config /config/input/config.json --path /music
```

`/config/onetagger` is where the container stores `onetagger.log` and any cached Spotify token data.
If you already have a OneTagger state directory you want to reuse, bind that directory there instead of an empty folder.

The included `compose.yml` targets the same GHCR image:

```sh
ONETAGGER_CONFIG_FILE=/absolute/path/to/config.json \
ONETAGGER_MUSIC_DIR=/absolute/path/to/music \
ONETAGGER_STATE_DIR=/absolute/path/to/onetagger-state \
docker compose run --rm onetagger \
  autotagger --config /config/input/config.json --path /music
```

The CLI also supports auto rename through the `renamer` subcommand:

```sh
docker run --rm \
  -e PUID="$(id -u)" \
  -e PGID="$(id -g)" \
  -v /path/to/music:/music \
  -v /path/to/onetagger-state:/config/onetagger \
  ghcr.io/marekkon5/onetagger-cli:latest \
  renamer --path /music --template "%artist% - %title%" --preview
```

Drop `--preview` to apply the rename, add `--output /music-renamed` to write into another directory, or `--copy` to keep the source files in place.

If you need a custom npm registry for local image builds, pass your global npm config as a BuildKit secret:

```sh
docker build \
  --secret id=npmrc,src="$HOME/.npmrc" \
  --build-arg GITHUB_SHA="$(git rev-parse HEAD)" \
  -t onetagger-cli .
```


## Credits
Bas Curtiz - UI, Idea, Help  
SongRec (Shazam support) - https://github.com/marin-m/SongRec

## Support
You can support this project by donating on [PayPal](https://paypal.me/marekkon5) or [Patreon](https://www.patreon.com/onetagger)

## Compilling

### Linux & Mac
Install dependencies: [rustup](https://rustup.rs), [node](https://nodejs.org/en/download/package-manager/), [pnpm](https://pnpm.io/installation)

**Install remaining dependencies**
```
sudo apt install -y lld autogen libasound2-dev pkg-config make libssl-dev gcc g++ curl wget git libwebkit2gtk-4.1-dev
```

**Compile UI**
```
cd client
pnpm i
pnpm run build
cd ..
```

**Compile**
```
cargo build --release
```
Output will be in: `target/release/onetagger`


### Windows
You need to install dependencies: [rustup](https://rustup.rs), [nodejs](https://nodejs.org/en/download/), [Visual Studio 2019 Build Tools](https://aka.ms/vs/16/release/vs_buildtools.exe), [pnpm](https://pnpm.io/installation)

**Compile UI:**
```
cd client
pnpm i
pnpm run build
cd ..
```

**Compile OneTagger:**
```
cargo build --release
```

Output will be inside `target\release` folder.
