# LocateHub Deployment Guide

This guide covers LocateHub Local Mode, Connected Mode, and Self-Hosted Server Mode.

LocateHub is vibe-coded software. It has not had a manual third-party security audit. Do not expose a LocateHub Host Server directly to the public internet unless a qualified security review and hardening pass have been completed. Prefer trusted LAN, VPN, or private network access.

LocateHub is currently built for Manitoba Click Before You Dig locate workflows. Manitoba ticket parsing, response handling, dig permit handling, package building, and locate-area map overlays are the primary supported workflow. Other ticket sources or provinces should be treated as unsupported or limited until that format has been specifically added and tested.

## 1. Local Mode

Use Local Mode when one Windows computer will manage LocateHub data locally.

### Best For

- Single-user desktop workflow.
- Offline work.
- Testing LocateHub before setting up a server.
- Keeping Manitoba Click Before You Dig tickets, responses, permits, and packages on one machine.

### Setup

1. Download `LocateHub_Setup_<version>.exe` from the latest release.
2. Install LocateHub.
3. On first startup, choose **Local Mode**.
4. Import or create jobs/tickets.
5. Configure backups in Settings.

### Data Location

Local Mode stores data under the Windows user app-data folder, including database, jobs, packages, logs, and backups.

### Certificates

Local Mode does not normally require certificates because it does not need a network server.

### Security Notes

Local Mode is the lowest-risk mode because it does not expose a server to the network. It is still vibe-coded software, so keep backups and verify all imported data and generated job packages.

## 2. Connected Mode

Use Connected Mode when this desktop app should connect to an existing LocateHub Host Server.

### Best For

- Office/client computers.
- Users who should work from a central server database.
- Desktop users who need the richer Windows interface but should not directly access server files or SQLite.

### Requirements

- A running LocateHub Host Server.
- A valid LocateHub login.
- Network access to the Host Server URL.
- Certificate trust if using HTTPS with a self-signed certificate.

### Setup

1. Install LocateHub on the client computer.
2. On first startup, choose **Connect to Server**.
3. Enter the Host Server URL, for example:

   ```text
   https://192.168.2.53:8000
   ```

4. Test the connection.
5. If prompted for a self-signed certificate, trust it only if you control that server.
6. Log in with your LocateHub username/password.

### Certificates In Connected Mode

If the Host Server uses a public certificate, connect using the certificate hostname:

```text
https://locatehub.example.com:8000
```

If the Host Server uses a self-signed certificate, every client device must trust that certificate.

Important:

- A certificate for `locatehub.example.com` will not validate if you connect to `https://192.168.x.x`.
- A self-signed certificate for `192.168.2.53` must be installed/trusted on each Windows or Android client.
- Android requires installing the certificate as a CA/user certificate before HTTPS requests will succeed.

### Security Notes

Connected Mode should be used on a trusted LAN or VPN. Because LocateHub is vibe-coded and has not had a manual security audit, do not connect clients over the public internet to an exposed Host Server.

## 3. Self-Hosted Server Mode

Use Self-Hosted Server Mode when this Windows computer should run the LocateHub Host Server for desktop, browser, and Android clients.

### Best For

- Small office LAN setup.
- A Windows Server or dedicated office PC.
- Central database/files with multiple clients.
- Browser fallback access.
- Android client testing.

### Requirements

- Windows desktop/server machine.
- LocateHub installed.
- Firewall allowance for the chosen port, usually `8000`.
- Admin account created during first Host Server setup.
- Backups configured.

### Basic Setup

1. Install LocateHub.
2. Choose **Host Server From This Computer**.
3. Pick host options:
   - `127.0.0.1` for local-only testing.
   - `0.0.0.0` for LAN access.
4. Choose a port, usually `8000`.
5. Create the first admin account in the browser setup page.
6. Open Host Control Center.
7. Confirm `/api/health` and `/api/discovery` work.
8. Test from another desktop/mobile device.

### NSSM Service Mode

LocateHub can install/repair/remove a Windows service using bundled NSSM support.

Use service mode when the Host Server should:

- start after reboot;
- run without the desktop window staying open;
- keep logs available for support;
- behave more like a small office server.

After changing host URL, port, HTTPS certificate, or config values, repair/restart the service so NSSM uses the current settings.

### Server URL Choices

For LAN HTTP testing:

```text
http://192.168.2.53:8000
```

For LAN HTTPS testing with self-signed cert:

```text
https://192.168.2.53:8000
```

For a public/trusted certificate using a domain:

```text
https://locatehub.example.com:8000
```

The URL users enter must match the certificate identity.

### Certificates In Self-Hosted Mode

LocateHub supports:

- generated self-signed certificates;
- imported existing certificate/key pairs;
- public certificates issued by providers such as Let's Encrypt using DNS validation.

LocateHub expects:

- certificate file in PEM format;
- unencrypted private key file in PEM format;
- both paths saved in settings/config;
- secure cookies enabled for HTTPS.

Relevant config values:

```ini
LOCATEHUB_SSL_CERTFILE = C:\Path\To\fullchain.pem
LOCATEHUB_SSL_KEYFILE = C:\Path\To\privkey.pem
LOCATEHUB_COOKIE_SECURE = true
LOCATEHUB_ALLOWED_HOSTS = 127.0.0.1,localhost,192.168.2.53,locatehub.example.com
```

After importing/changing certificates:

1. Save settings.
2. Restart the Host Server.
3. If using NSSM, use **Repair Service** or restart the service.
4. Test `/api/health`.
5. Test one desktop client and one Android client if mobile is in use.

### Public Domain Certificates

A public certificate can remove self-signed certificate warnings. A common approach is DNS-01 validation through Cloudflare or another DNS provider.

For example:

- domain: `locatehub.example.com`;
- certificate covers: `locatehub.example.com`;
- clients connect to: `https://locatehub.example.com:8000`;
- DNS points to the server only if your network design supports it.

Even with a public certificate, LocateHub is not recommended for direct internet exposure because it is vibe-coded and has not had a manual security audit. A certificate only proves identity and encrypts traffic; it does not make the application internet-hardened.

### Firewall

For LAN access, allow inbound TCP traffic on the configured port, usually `8000`.

Only open the port on trusted network profiles if possible.

### Backups

Before updating a Host Server:

1. Create a full backup.
2. Confirm backup location.
3. Install update.
4. Confirm Host Server health.
5. Confirm desktop and Android clients can reconnect.

## 4. Updates

Use these files from the latest GitHub Release:

- Installer: `LocateHub_Setup_<version>.exe`
- Updater ZIP: `LocatesTracker.<version>.zip`
- Android APK: `LocateHub-Mobile-<version>.apk`

The in-app updater uses the ZIP. First-time installs use the EXE.

For Host Server updates, use the Host Control Center update path where possible. It preserves app data, restarts managed server processes/services, and validates health after installation.

## 5. Android Client Notes

The Android app connects to the Host Server API. It does not access server folders or SQLite.

For Android HTTPS:

- public certificates normally work without extra steps if the device trusts the issuing CA;
- self-signed certificates must be installed on the Android device;
- the URL must match the certificate name/IP.

Android should be tested on the same network as the Host Server before field use.

## 6. Troubleshooting

### Cannot Connect

Check:

- Host Server is running.
- Correct URL and port.
- Firewall allows the port.
- `/api/health` works in a browser.
- Certificate is trusted if using HTTPS.
- Client is using the exact hostname/IP covered by the certificate.

### HTTPS Certificate Error

Check:

- certificate file and private key match;
- private key is unencrypted PEM;
- server was restarted after changing cert settings;
- Windows/Android trusts the certificate;
- URL matches certificate SAN/CN.

### Update Rolls Back

Check:

- Host Server is reachable after restart;
- `/api/health` returns `status: ok`;
- certificate trust does not block the update health check;
- logs in `%LOCALAPPDATA%\LocateHub\logs`.

### Browser Works But App Does Not

Check:

- API login is valid;
- token/session did not expire;
- certificate is trusted by the client app/device;
- desktop/mobile app is using the same URL that works in the browser.

## 7. Production Caution

LocateHub can be useful on a trusted LAN, but it should not be treated as a hardened internet-facing product.

Because LocateHub is vibe-coded and has not had a manual security audit:

- do not expose it directly to the public internet;
- do not skip backups;
- use strong passwords;
- restrict firewall rules;
- prefer VPN/private network access;
- validate every release in a test workflow before depending on it.
