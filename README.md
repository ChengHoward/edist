# edist

Custom Electron binaries distributed through GitHub Releases.

Each release contains platform ZIP archives, SHASUMS256.txt, and the corresponding Electron npm package tarball.

To use a custom release, install its electron-VERSION.tgz asset and configure the following environment variables before npm install:

```powershell
$env:ELECTRON_MIRROR = "https://github.com/ChengHoward/edist/releases/download/"
$env:ELECTRON_CUSTOM_DIR = "v{{ version }}"
$env:ELECTRON_USE_REMOTE_CHECKSUMS = "1"
```

Example dependency for version 44.0.0-cloudbypass.6:

```json
{
  "devDependencies": {
    "electron": "https://github.com/ChengHoward/edist/releases/download/v44.0.0-cloudbypass.6/electron-44.0.0-cloudbypass.6.tgz"
  }
}
```

Available platforms vary by release. Refer to the assets attached to the selected release.