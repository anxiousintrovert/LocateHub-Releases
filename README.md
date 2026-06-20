# LocateHub Releases

LocateHub is a vibe-coded Windows locate-ticket management app for jobs, locate tickets, dig permits, utility responses, job packages, file storage, backups, and lightweight team access.

This repository is for public release downloads and user documentation. Source code is not published here.

## Important Notice

LocateHub is vibe-coded software. It was built through an iterative AI-assisted workflow and is provided as-is.

LocateHub has not had a manual third-party security audit. It is not recommended to expose a LocateHub Host Server directly to the public internet. Use it on a trusted local network or behind a properly secured private network/VPN unless a qualified security review is completed.

Always verify tickets, permits, responses, expiry dates, packages, and map/locate-area overlays before relying on them for field or excavation decisions.

## Downloading LocateHub

Use the latest GitHub Release from this repository:

- `LocateHub_Setup_<version>.exe`  
  First-time Windows installer.

- `LocatesTracker.<version>.zip`  
  In-app updater package.

- `LocateHub-Mobile-<version>.apk`  
  Android companion app package for testing/field status workflows.

- `.sha256` files  
  Checksums for verifying downloads.

Do not use old build files committed to repository history. Current builds are published as GitHub Release assets.

## Operating Modes

LocateHub supports three main desktop modes:

- **Local Mode**: one Windows computer, local data, offline-capable.
- **Connected Mode**: desktop app connects to an existing LocateHub Host Server.
- **Self-Hosted Server Mode**: this computer hosts the LocateHub server for other desktop/mobile/web clients on the LAN.

See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) for setup instructions.

## Android App

The Android app is a client for a LocateHub Host Server. It does not access SQLite or job folders directly.

Use it to:

- connect to a LocateHub Host Server;
- log in with an API user;
- view assigned jobs;
- view job tickets, dig permits, responses, packages, and map overlays where supported;
- update response status/date/notes.

For HTTPS with a self-signed Host Server certificate, Android must trust the server certificate and the app must connect using the exact hostname or IP covered by that certificate.

## Backups

Before installing updates:

1. Create a LocateHub backup.
2. Confirm the update version is newer than your installed version.
3. Keep a copy of important job folders and packages if the data is business-critical.
4. Test the updated app before relying on it in production workflow.

## Security Summary

Recommended:

- Use Local Mode for single-user/offline use.
- Use Self-Hosted Server Mode only on trusted LAN/VPN networks.
- Use HTTPS when connecting from other devices.
- Use strong unique admin passwords.
- Keep backups.
- Keep Windows and root certificates updated.

Not recommended:

- Exposing the Host Server directly to the internet.
- Running with default secrets/passwords.
- Treating generated packages or parsed ticket data as a substitute for human verification.

## Support Notes

When reporting issues, include:

- LocateHub version.
- Mode: Local, Connected, or Self-Hosted.
- Whether HTTP or HTTPS is used.
- Relevant screenshots.
- Support logs from the Host Control Center when available.
- Whether Android, desktop, or browser access is affected.
