# card-extractor — Release Distribution

Release artifacts for **PDF 거래내역 추출기** (a desktop app that extracts Korean bank
and credit-card statement PDFs into Excel). The source code lives in a separate
private repository.

## Why the split

The app checks this repository's **GitHub Releases** for updates via
`electron-updater`. Releases in a private repository require a token to download,
and any token shipped inside the app can be extracted by its users — which makes it
public in practice. So the code stays private and only the build artifacts are
published here.

## Release contents

| File | Purpose |
|---|---|
| `card-extractor-setup-<version>.exe` | NSIS installer — the auto-update target |
| `card-extractor-setup-<version>.exe.blockmap` | Block map for differential downloads |
| `card-extractor-<version>.exe` | Portable build (no installation) |
| `latest.yml` | Update metadata the updater reads (includes sha512 and size) |

`latest.yml` carries the installer's sha512 and byte size. If they disagree with the
uploaded `.exe`, the updater fails verification and silently refuses the update —
so **always publish all three from the same build**.

## How releases are published

From the source repository:

```bash
cd electron
GH_TOKEN=<token with Contents: write on this repo> npm run build:win -- --publish always
```

`electron-builder` creates a **draft** release and normalizes asset names to ASCII
(the local build output uses a Korean filename; GitHub assets do not). Review the
draft, then press **Publish release** to make it visible to the updater.

## How the app updates

- Checks once, silently, ~15 seconds after launch, and on demand via
  **File → 업데이트 확인**
- The silent check only speaks up when a newer version exists — being up to date or
  failing to reach GitHub produces no dialog
- Downloads only after the user agrees; installs on restart or on app quit
