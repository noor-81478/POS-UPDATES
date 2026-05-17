# MHSentryPOS — GitHub Pages Update System

This directory contains the manifest files served via GitHub Pages for the auto-update system.

## Setup

1. **Create a GitHub repository** (e.g., `mhsentrypos-updates`)
2. **Enable GitHub Pages** in repository Settings → Pages → Source: `gh-pages` branch
3. **Push the `github-pages/` contents** to the `gh-pages` branch

## Directory Structure

```
clients/
├── client-a/
│   └── manifest.json
├── client-b/
│   └── manifest.json
└── <client-id>/
    └── manifest.json
```

Each client has its own manifest, allowing:
- Different versions per client
- Individual blocking (e.g., unpaid license)
- Custom messages per client

## Manifest Schema

```json
{
  "status": "active",
  "version": "2.0.0",
  "download_url": "https://github.com/<owner>/<repo>/releases/download/v2.0.0/MHSentryPOS.jar",
  "sha256": "<sha256-hex-checksum-of-jar>",
  "message": "What's new in this version"
}
```

| Field          | Type   | Values                | Description                                    |
|----------------|--------|-----------------------|------------------------------------------------|
| `status`       | string | `"active"`, `"blocked"` | If blocked, app shows message and exits       |
| `version`      | string | Semantic version       | e.g., `"1.0.0"`, `"2.1.3"`                   |
| `download_url` | string | Full URL               | Direct download link to the JAR file          |
| `sha256`       | string | 64-char hex            | SHA-256 checksum of the JAR for verification  |
| `message`      | string | Any text               | Shown as update notes or blocked reason       |

## How to Block a Client

Set `status` to `"blocked"` and provide a `message`:

```json
{
  "status": "blocked",
  "version": "1.0.0",
  "download_url": "",
  "sha256": "",
  "message": "Your license has expired. Please contact support."
}
```

## Client App Configuration

Each client app needs these settings in `~/.mystore/config.properties`:

```properties
app.version=1.0.0
client.id=client-a
update.manifest.url=https://<username>.github.io/<repo>/clients/client-a/manifest.json
```

## Computing SHA-256

**Windows (PowerShell):**
```powershell
Get-FileHash -Algorithm SHA256 .\MHSentryPOS.jar | Select-Object -ExpandProperty Hash
```

**Linux/Mac:**
```bash
sha256sum MHSentryPOS.jar
```
