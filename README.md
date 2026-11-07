# Custom Electron distributions

## cloudbypass

Foundation customizations: UA / Client Hints, networking, trusted input, DOM automation, file input, and JavaScript dialogs.

Current release: [45.0.0-cloudbypass.2](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-cloudbypass.2). Available binaries: Windows x64 and Linux x64. Linux evidence is primarily static inspection; do not infer full runtime parity. No macOS package is published for this release.

## desktop

Includes the cloudbypass foundation plus native panels, menus, page information, certificate UI, virtual cursor, and desktop interaction enhancements.

Current release: [45.0.0-desktop.3](https://github.com/ChengHoward/edist/releases/tag/v45.0.0-desktop.3). Available binary: Windows x64 only. Adds native scrollbar corner avoidance; upstream scrollbar appearance is unchanged. desktop.2 was the unchanged-binary rename of cloudbypass.11 and remains available.

## Installation

The npm package name remains `electron`. Select an exact distribution version; the two editions have independent release numbers.

```ini
electron_mirror=http://127.0.0.1:8787/electron/
electron_custom_dir=v{{ version }}
electron_use_remote_checksums=1
```

```powershell
# cloudbypass
npm install --save-dev http://127.0.0.1:8787/electron/npm/electron/-/electron-45.0.0-cloudbypass.2.tgz
# desktop (Windows x64)
npm install --save-dev http://127.0.0.1:8787/electron/npm/electron/-/electron-45.0.0-desktop.3.tgz
```

GitHub npm packages: [cloudbypass](https://github.com/ChengHoward/edist/releases/download/v45.0.0-cloudbypass.2/electron-45.0.0-cloudbypass.2.tgz), [desktop](https://github.com/ChengHoward/edist/releases/download/v45.0.0-desktop.3/electron-45.0.0-desktop.3.tgz).

## Historical releases

Earlier releases remain available for compatibility. Versions 45.0.0-cloudbypass.3 through .11 are legacy combined distributions; they do not advance the foundation-only cloudbypass release beyond .2. Historical manifests and reports retain their original identifiers.

## Local mirror

Start the local service from the mirror directory with `npm run serve:local`. The optional existing tunnel serves the same files at https://mirror.cloudbypass.com/electron/ . Local service health: http://127.0.0.1:8787/electron/healthz .

Registry tags: `cloudbypass` points to 45.0.0-cloudbypass.2; `desktop` and `latest` point to 45.0.0-desktop.3. Historical exact-version URLs remain available.
