# Bengal Download Manager

[![Flathub](https://img.shields.io/flathub/v/bd.com.zihad.BengalDownloadManager?style=flat-square&logo=flathub&logoColor=white&label=flathub)](https://flathub.org/apps/bd.com.zihad.BengalDownloadManager)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](https://github.com/tazihad/bengal-download-manager/blob/main/LICENSE)
[![Platform: Linux](https://img.shields.io/badge/platform-Linux-8b5cf6?style=flat-square)](#installation)

Flathub manifest for **Bengal Download Manager**, a multi-threaded download manager
for Linux - a downloader with segmented
transfers, pause/resume, browser integration, and a media/site grabber.

[![Bengal Download Manager](https://dl.flathub.org/media/bd/com/zihad.BengalDownloadManager/49adafc2532f6cddc9810036237ed990/screenshots/image-1_orig.png)](https://flathub.org/apps/bd.com.zihad.BengalDownloadManager)

- **Upstream project:** <https://github.com/tazihad/bengal-download-manager>
- **Website:** <https://zihad.com.bd/bengal-download-manager/>
- **Flathub page:** <https://flathub.org/apps/bd.com.zihad.BengalDownloadManager>

## Installation

[Set up Flathub](https://flatpak.org/setup/) first, then install the app:

```sh
flatpak install flathub bd.com.zihad.BengalDownloadManager
```

Run it with:

```sh
flatpak run bd.com.zihad.BengalDownloadManager
```

## Permissions

The manifest requests the following sandbox permissions:

| Permission | Why it is needed |
| --- | --- |
| `--share=network` | Downloading files and talking to the local aria2/browser-extension IPC. |
| `--share=ipc` | Shared-memory IPC between the app and aria2. |
| `--socket=wayland`, `--socket=fallback-x11` | Native display on Wayland, X11 as a fallback. |
| `--device=dri` | GPU access for Qt rendering. |
| `--filesystem=xdg-download` | Saving completed downloads to the user's Downloads folder. |
| `--talk-name=org.freedesktop.FileManager1` | "Open containing folder" from the downloads list. |
| `--talk-name=org.kde.StatusNotifierWatcher`, `--talk-name=org.freedesktop.StatusNotifierWatcher` | Tray icon and notifications on KDE and other desktops. |

These can be narrowed or extended locally with `flatpak override`, e.g.:

```sh
# Give access to an extra folder for downloads
flatpak override --user --filesystem=/path/to/folder bd.com.zihad.BengalDownloadManager
```

## Building locally

The manifest builds the upstream release tarball with CMake + PyInstaller on top of
`com.riverbankcomputing.PyQt.BaseApp`, with Qt from the KDE runtime.

```sh
# Add the Flathub remote (skip if already configured)
flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo

# Build and install locally
flatpak-builder --user --install --force-clean \
  --install-deps-from=flathub \
  build-dir bd.com.zihad.BengalDownloadManager.yml

# Run the local build
flatpak run bd.com.zihad.BengalDownloadManager
```

`--install-deps-from=flathub` pulls in the required `org.kde.Platform`/`org.kde.Sdk`
6.11 runtime and the PyQt BaseApp automatically.

## Manifest layout

| File | Purpose |
| --- | --- |
| `bd.com.zihad.BengalDownloadManager.yml` | Main manifest: runtime, base app, sandbox permissions, and the upstream source archive with its checksum. |
| `python3-requirements.yml` | Bundled Python dependencies for the build (PyInstaller, `python-socks`, `bgutil-ytdlp-pot-provider`). |

## Updating the manifest

Flathub updates are opened automatically by [flathub-bot](https://github.com/flathub-infra/flathub-external-data-checker)
when a new upstream tag is published. To update manually, bump the version in the
archive URL and refresh the checksum:

```sh
curl -L -o source.tar.gz \
  https://github.com/tazihad/bengal-download-manager/archive/refs/tags/vX.Y.Z.tar.gz
sha256sum source.tar.gz
```

Then update `url` and `sha256` in the manifest and verify the build as described
above before opening a pull request.

## Reporting issues

- **Flatpak packaging problems** (sandbox permissions, missing integrations,
  build failures): open an issue in this repository.
- **App bugs and feature requests**: report them upstream at
  <https://github.com/tazihad/bengal-download-manager/issues>.

## License

Bengal Download Manager is released under the [MIT License](https://github.com/tazihad/bengal-download-manager/blob/main/LICENSE).
This packaging repository follows the same license.