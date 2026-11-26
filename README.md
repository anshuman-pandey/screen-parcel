<div align="center">

![Screen Parcel — show it once, keep the file, share on your terms](docs/assets/banner.svg)

### Open-source alternative to Loom for screen recording and self-hosted sharing

Record a walkthrough. Keep a local copy. Share a link you can revoke.

**MIT licensed** · **Single owner** · **Python or Docker** · **No third-party video storage**

[**Run locally →**](#run-it-on-your-computer) · [See the workflow](#record-preview-share-revoke) · [Self-hosting guide](docs/operations.md#self-host) · [Launch checklist](HANDOFF.md)

</div>

> **Prepared preview, not a launched service.** The local recording and sharing workflow passes automated acceptance with synthetic video. Real screen-picker interaction, microphone hardware and hosted HTTPS still need manual verification. There is no live public demo yet.

![Screen Parcel recording workspace with preview, local download and revocable sharing controls](docs/assets/studio.png)

*Actual application screenshot from container acceptance. The video is a clearly labeled synthetic test recording, not customer footage.*

## Record. Preview. Share. Revoke.

| 1 · Record | 2 · Keep a copy | 3 · Share deliberately | 4 · Control future access |
|---|---|---|---|
| Choose a screen, window or tab. Optional microphone narration. | Preview the result and download the file. | Enter your owner key and upload to your own server. | Revoke the viewer link and delete the hosted recording. |

Nothing uploads when you press Record. You choose whether to keep a local file or create a hosted link. Anyone with a shared link can watch and download it; revoking a link cannot recall a downloaded copy.

## Run it on your computer

You need **Python 3.12+**. Extract the source package, then run these commands from its folder:

```sh
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
export OWNER_TOKEN="$(python3 -c 'import secrets; print(secrets.token_urlsafe(32))')"
export DATA_DIR=./data
python app.py
```

Open **http://127.0.0.1:5120**. Select **Choose screen & record**, choose non-private test content in the browser picker, then stop and preview. You can download without an owner key. Uploading or managing the library requires the `OWNER_TOKEN` generated in your terminal; retain it privately for subsequent launches.

These commands are for macOS/Linux shells. The [launch handoff](HANDOFF.md) covers key handling, prerequisites and exact acceptance steps. This is a server-backed app, not a double-click HTML file. Native Windows launcher and one-click setup are not included yet.

**Already have a recording?** Import a browser-supported WebM or MP4 and exercise preview → upload → viewer → revoke. The browser suite also creates a playable synthetic recording to verify the complete flow without screen permissions.

<details>
<summary><strong>Prefer Docker?</strong></summary>

With Docker installed and `OWNER_TOKEN` set in your shell:

```sh
docker build -t clip-cabinet .
docker volume create clip-cabinet-data
docker run --rm -p 127.0.0.1:5120:5120 -e OWNER_TOKEN \
  -v clip-cabinet-data:/app/data clip-cabinet
```

The named volume retains your library. The container runs as UID 10001. See the [operations guide](docs/operations.md#self-host) for HTTPS, upload limits, backup, restore and trusted-CA builds.

</details>

## A viewer focused on the recording

Recipients see a title, playback controls and a download. They do not need your owner key. Viewer links are unlisted bearer links, not authenticated team access.

![Screen Parcel viewer with video playback and download](docs/assets/viewer.png)

[View the mobile layout](docs/assets/mobile.png) · [Read the recording and storage limits](docs/operations.md#run-locally)

## What this replaces—and what it doesn't

Screen Parcel targets Loom's **record → preview → share a link** workflow. It is a smaller, self-hosted tool, with no claim of feature parity or affiliation.

| Included | Not included |
|---|---|
| Browser screen recording and optional microphone | Camera overlay or system audio |
| Local preview and download; WebM/MP4 import | Video editing or transcription |
| Single-owner uploads and revocable viewer links | Team accounts, comments or analytics |
| Local SQLite/file storage; Docker packaging | Managed hosting or cloud sync |
| Five-minute recording limit; 100 MB upload cap | Unlimited recording or storage |

**No tracking scripts or third-party video uploads.** Keep owner keys private, use HTTPS for remote recording, and review clips before sharing. [Deployment details and privacy limits →](docs/operations.md)

## Verify the whole workflow

After installing the Python requirements, install test dependencies:

```sh
pip install pytest==9.2.0
npm ci
npx playwright install chromium
python -m pytest -q
python scripts/check_local.py
```

The isolated runner generates a temporary owner key and library, starts a loopback server, runs the browser workflow, then cleans up. It does not use your existing recording library.

| Evidence | Status |
|---|---|
| Seven server tests | Passed in checkout and extracted package |
| Real MediaRecorder, download, upload, viewer playback and revoke | Passed with synthetic screen frames |
| Docker restart, backup and restore | Previously passed; not rerun for this documentation update |
| Real picker, microphone and hosted HTTPS | Manual checks outstanding |

[Validation receipt](artifacts/validation.json) · [Manual acceptance and launch handoff](HANDOFF.md)

---

[MIT license](LICENSE) · Your screen. Your file. Your server.

## Interface languages

English · Deutsch · Español · 日本語 · 简体中文 · 繁體中文

The interface follows your browser preferences until you choose a language. Switching keeps your draft and saves your choice locally. Translations are bundled; no translation service or telemetry is used. Your content is never translated. Unknown technical diagnostics fall back to English. See [localization notes](I18N.md) for scope and testing.

## Current verification

See the [26 November audit checkpoint](docs/AUDIT.md) for the verified repository, release links, fresh core test result and remaining deployment/browser gates. Earlier validation records remain dated evidence.
