# Screen Parcel — installation and recording acceptance

Public source preview at [anshuman-pandey/screen-parcel](https://github.com/anshuman-pandey/screen-parcel). Hosted deployment and a tested product release remain unverified. Open-source alternative to Loom for browser screen recording, local playback/download and single-owner, self-hosted revocable video links. MIT licensed. No camera overlay, editing, transcription, comments, teams or system-audio capture.

## Runnable artifacts

- Download the repository’s **Code → Download ZIP** or clone `https://github.com/anshuman-pandey/screen-parcel.git`, then follow the Python startup below. This is a web app with a local server, not a double-click HTML app or prebuilt binary.
- `Dockerfile`: buildable container definition; no published container image is claimed.
- `scripts/check_local.py`: repeatable isolated browser acceptance runner after installing Python and Node test dependencies. It chooses a free loopback port, generates a temporary key/library, runs the browser suite and cleans up its server and data. It never targets the owner's library.
- `scripts/docker_acceptance.py`: existing isolated container persistence/backup/restore acceptance.

## Start the source preview

From the extracted folder, in a terminal:

```sh
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
export OWNER_TOKEN="$(python3 -c 'import secrets; print(secrets.token_urlsafe(32))')"
export DATA_DIR=./data
python app.py
```

Open http://127.0.0.1:5120 in Chrome. In another terminal you will not inherit that token: retain it in your private password manager before closing the original shell, or stop the app temporarily and inspect `OWNER_TOKEN` in that original shell. Do not share it in chat or screenshots. You can also set OWNER_TOKEN to a previously saved long random value before launching. The key grants full library access; it is only required for upload/manage, not local recording.

## Historical local checks (see current audit for newer evidence)

- Seven server tests passed in the existing checkout.
- Isolated local acceptance passed: synthetic screen frames → real MediaRecorder → local playback → download → authenticated upload → viewer playback in a separate page → revoke and verify 404.
- File import, missing-key recovery and simulated permission-denial recovery passed; no browser script errors.
- Historical extracted ZIP checks used the same server and browser suites; this does not verify a newly downloaded archive.
- Existing November 15, 2026 IST evidence records non-root Docker, byte ranges, restart persistence, revocation across restart, stopped-volume backup/restore and post-restore writes. These container checks were **not rerun in this handoff**; application runtime code has not changed.
- Remote CI has since passed; see the [current audit](docs/AUDIT.md) and [Actions](https://github.com/anshuman-pandey/screen-parcel/actions). No public server, hosted link or tested product release is claimed.

## Manual release checks

Use only synthetic content. Screen permission and actual microphone operation have not been verified by automated tests.

1. Start the app above. Click Choose screen & record. In the **real browser picker**, choose a tab/window displaying non-private test text. Change that text or scroll, then stop. Confirm the preview and downloaded file show those changes.
2. Test canceling the real picker. Confirm the app recovers and can start a later recording. The automated denial check uses a mock and is not a substitute for this step.
3. Turn on microphone narration, grant the browser permission, say a test sentence and stop. Play the downloaded file and confirm narration is audible. If no microphone is available, report that check as unverified.
4. Enter the owner key, upload the test recording and open its viewer link in an incognito window on the same computer. Confirm playback/seeking without an owner key. A localhost URL is not reachable by someone on a different computer.
5. Revoke & delete that recording; refresh its incognito viewer and confirm 404. Downloaded copies cannot be recalled.
6. The owner authorized continued completion and deployment on 26 November 2026. Hosted HTTPS still requires a configured production host and a repeat of picker/upload/viewer/revoke checks on the actual HTTPS origin. Do not treat localhost success as hosted acceptance.

Record browser/OS, the picker choice, whether video and microphone passed, and any exact error. Do not paste owner keys or private recordings.

## Exact next action

Run the source preview and complete the real picker/microphone checks above. Record failures or unavailable hardware honestly. The independent repository already exists; no creation form or repeated source-push authorization is needed. Continue tested publication under the standing authorization when the remaining release gates pass. Production hosting still needs an appropriate persistent host, credentials and HTTPS origin. Broader naming review remains pending.
