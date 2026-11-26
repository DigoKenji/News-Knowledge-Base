---
type: Article Summary
title: 15 self-hosted apps that quietly replaced my paid subscriptions
description: Fifteen self-hosted apps on one small box can replace most paid subscriptions, but only if you accept running
  and backing up the server.
resource: https://medium.com/@sarathm09/15-self-hosted-apps-that-quietly-replaced-my-paid-subscriptions-f84d6d4fc712
tags:
- software-engineering
- virtualization-containers
- network-security
- encryption
- least-privilege
- attack-surface
- large-language-model
- inference-tradeoffs
- database-dbms
- finops
- availability-reliability
- infrastructure-as-code
- reverse-proxy
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://medium.com/@sarathm09/15-self-hosted-apps-that-quietly-replaced-my-paid-subscriptions-f84d6d4fc712
  title: 15 self-hosted apps that quietly replaced my paid subscriptions
  author: Sarath S
status: stable
source_url: https://medium.com/@sarathm09/15-self-hosted-apps-that-quietly-replaced-my-paid-subscriptions-f84d6d4fc712
source: Medium (Sarath S)
source_type: medium
published: '2026-10-18'
domain: software-engineering
concepts:
- virtualization-containers
- network-security
- encryption
- least-privilege
- attack-surface
- large-language-model
- inference-tradeoffs
- database-dbms
- finops
- availability-reliability
- infrastructure-as-code
- reverse-proxy
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d81899744e60653f6f4e6
---

## 🎯 Core thesis
Fifteen self-hosted replacements for paid subscriptions can run on one mini PC and cut about \$125 a month at list price, but only if you accept the job of running and backing up the server yourself.

## 🧭 Why the author wrote it
Small recurring charges for storage, passwords, AI chat and similar tools add up without anyone noticing; this author's list totals \$125.57 a month. If the thesis holds, one inexpensive home server replaces most of those bills and pays for its hardware within months, while you take on the role of system administrator.

## Opening: the subscriptions and the plan
The author adds up the list prices and gets \$125.57 a month. ChatGPT Plus and Zapier are about \$20 each and Acrobat is \$14.99; most of the rest cost \$1.65 to \$12 each. Two count as \$0: Home Assistant replaces free apps, and the author could not confirm a US price for Evernote. Fourteen replacements run in Docker on one small box, and the last runs with a single `bunx` command. The author runs Pi-hole and built mdb (entry 15); the other thirteen were checked against recent releases and US prices as of October 1, 2026. The hardware figures describe a representative build, not one the author measured.

### Small recurring charges
Each charge is small enough to ignore, which is how they pile up. The list runs from the most familiar bills (photos and files) to niche work tools. Every entry follows the same pattern: what the paid app does for you, the replacement, the monthly saving, the command to start it, and one downside.

## What you need first
### A mini PC
Any mini PC with an Intel N100 or N150 and 16GB of RAM covers the list, according to the author, who prices one build in the math section. The box runs Ubuntu Server or Debian.

### Docker and Compose
Docker runs each app as a container, and Docker Compose describes a multi-container app in one YAML file. Install Docker Engine from Docker's guide, then run `sudo usermod -aG docker $USER` and log back in so Docker works without sudo.

> 🔎 Added context: membership in the `docker` group gives a user control over the Docker daemon, which is effectively root-level power on the host. Treat that account accordingly.

### Stable addresses and one data folder
Reserve the box's IP address in the router's DHCP settings so it never changes; every web UI in the article is reached at `http://<server-ip>:<port>`. Keep all compose folders and app data under `/srv`, so one backup of `/srv` covers most of it. Named Docker volumes live under `/var/lib/docker/volumes`, which is why the backup in the final section includes that path.

### Outside access through a VPN
Reach the box from outside your network through a VPN such as Tailscale or WireGuard, not by opening ports on the router.

> ❌ **Anti-pattern — forwarding ports to the box:** opening ports on the router so the internet can reach a self-hosted app directly. **Why people do it:** it is the quickest way to get remote access working. **Consequence:** every exposed service becomes a target on the open internet. **Fix:** reach the box through a VPN, as the article recommends.

## Photos and files
The author starts with the phone. Photos fill free cloud storage within a year or two, and documents, calendars and contacts follow the photos home.

### 1. Immich (replaces iCloud+ or Google One storage, \$9.99/mo)
iCloud gives 5GB free and Google gives 15GB. A few years of photos and video exceed both, so people pay for the 2TB tier of iCloud+ or Google One, \$9.99 a month either way, mostly so the camera roll backs up on its own. Immich is the closest self-hosted match for Google Photos: its phone app backs up in the background, and its web app offers a timeline, face recognition, a map view and shared albums.

```bash
mkdir -p /srv/immich-app && cd /srv/immich-app
wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
# edit .env: change DB_PASSWORD; if UPLOAD_LOCATION points at a separate photo drive, back that drive up too
docker compose up -d   # web UI on :2283
```

**What this does:**
- `mkdir -p` and `cd` create a folder under `/srv`, so the compose file and its data sit in the path the backup covers.
- The two `wget` lines download the project's official compose file and example environment file from the latest release.
- The comment says to change `DB_PASSWORD` in `.env` and to back up a separate photo drive if `UPLOAD_LOCATION` points at one.
- `docker compose up -d` starts the containers in the background; the web UI listens on port 2283.

Immich ships fast: v3.2.4 landed on September 28, 2026, and major versions bring breaking changes. Read the release notes before every upgrade.

> ❌ **Anti-pattern — one copy of the library:** keeping the only copy of the photo library on one drive. **Why people do it:** it is the default when the server is the first place photos go. **Consequence:** a failed drive loses the whole library. **Fix:** keep a second copy somewhere else, as the article advises.

### 2. Nextcloud (replaces Google Drive, Calendar and Contacts, \$1.99/mo)
Google Drive holds the lease and the scanned IDs, and Google keeps calendar and contacts in sync across devices. A few years of PDFs push past the free 15GB into Google One's 100GB plan at \$1.99 a month. If your files fit in the free tier this entry saves nothing, and if photos and files already share one Google One 2TB plan, count only the \$9.99 from entry 1.

Nextcloud combines files, calendar and contacts in one server, and phones and laptops sync to it the way they sync to Google. The author picks it over Syncthing, which syncs folders but has no calendar, contacts or web file interface, and over ownCloud, which became part of Kiteworks in 2023 and is replacing its PHP server with a Go rewrite; Nextcloud keeps the PHP server and its app ecosystem.

```bash
docker run --init --sig-proxy=false --name nextcloud-aio-mastercontainer --restart always \
  --publish 80:80 --publish 8080:8080 --publish 8443:8443 \
  --volume nextcloud_aio_mastercontainer:/mnt/docker-aio-config \
  --volume /var/run/docker.sock:/var/run/docker.sock:ro \
  ghcr.io/nextcloud-releases/all-in-one:latest
# setup UI on https://<server-ip>:8080
```

**What this does:**
- The command starts only AIO's "mastercontainer", which then installs and manages the Nextcloud containers.
- `--restart always` brings it back after a reboot.
- The three `--publish` flags open ports 80, 8080 and 8443; port 8080 serves the setup UI.
- The named volume keeps AIO's configuration across restarts.
- The Docker socket mount lets the container control other containers on the host.

> 🔎 Added context: the `:ro` suffix on the socket does not restrict what the Docker API allows, so it does not make the socket safe to expose.

Nextcloud is the heaviest item on the list and its major upgrades are the fussiest. AIO needs a domain and a valid certificate; the article points to AIO's local-instance guide for a free deSEC domain, Tailscale or the DNS challenge if you want LAN-only access.

> ❌ **Anti-pattern — mounting the Docker socket:** giving a web-facing management container read access to `/var/run/docker.sock`. **Why people do it:** the installer needs it to manage the other containers. **Consequence:** the article warns that this gives the container root-level control of the host. **Fix:** keep the box off the open internet and accept the trade-off knowingly.

## Paperwork
With files on the server, the paper pile is next: tax letters, warranties, statements and PDFs to sign and return.

### 3. Paperless-ngx (replaces Evernote's paid tier for scanned paper, counted as \$0)
People pay for Evernote to scan paper and search its text later, as in "where's the warranty for the fridge?" Evernote's pricing shows regional prices and the author could not confirm a US figure, so the math counts this entry as \$0. Paperless-ngx watches a "consume" folder: anything dropped in gets OCR'd, made searchable and tagged by sender and type.

```bash
bash -c "$(curl -L https://raw.githubusercontent.com/paperless-ngx/paperless-ngx/main/install-paperless-ngx.sh)"
# asks a few setup questions; give it /srv/paperless as the install folder; web UI on :8000 by default
```

**What this does:**
- `curl -L` downloads the project's install script and follows redirects.
- `bash -c "$(...)"` runs the script immediately without saving it first, so you are trusting that URL.
- The script asks setup questions; the comment says to answer `/srv/paperless` for the install folder, which keeps its data under `/srv`.
- The web UI listens on port 8000 by default.

The project's README says documents are stored in clear text, so do not run it on a host you do not trust.

### 4. Stirling-PDF (replaces Acrobat Standard, \$14.99/mo)
Acrobat Standard costs \$14.99 a month on an annual plan or \$24.99 month to month. Stirling-PDF handles merging, OCR and format conversion in the browser, and the files never leave your network. It is open-core, so some enterprise features cost money, and version 3.0 changed enough that older guides no longer match.

```bash
docker run -d -p 8085:8080 --restart unless-stopped docker.stirlingpdf.com/stirlingtools/stirling-pdf   # web UI on :8085, since Nextcloud AIO has 8080
```

**What this does:**
- `-d` runs the container in the background.
- `-p 8085:8080` maps host port 8085 to the app's port 8080, avoiding the clash with Nextcloud AIO.
- `--restart unless-stopped` restarts it after a reboot unless you stopped it by hand.
- The image comes from the project's own registry domain.

## Media
This group covers movies and audiobooks you already own, playable on any screen in the house.

### 5. Jellyfin (replaces Plex Pass and some streaming subscriptions, \$6.99/mo)
Plex turns a folder of ripped movies or home videos into a streaming-style library, but watching away from home is the paid part: Plex Pass is \$6.99 a month and the Remote Watch Pass is \$2.99. The table uses Plex Pass because it is the tier that lets the server's graphics chip transcode on the fly for phones and slow connections. Jellyfin offers the same library with posters and resume points and has no paid tier.

```bash
docker run -d --name jellyfin -p 8096:8096 \
  -v /srv/jellyfin/config:/config -v /srv/jellyfin/cache:/cache \
  -v /srv/media:/media --device /dev/dri:/dev/dri \
  --restart unless-stopped jellyfin/jellyfin   # /dev/dri lets the Intel iGPU transcode
# web UI on :8096
```

**What this does:**
- `-p 8096:8096` exposes the web UI.
- The two `/srv/jellyfin` mounts keep configuration and cache in the backed-up `/srv` tree.
- `-v /srv/media:/media` points the library at your media folder.
- `--device /dev/dri:/dev/dri` passes the Intel integrated GPU into the container so it can transcode.
- `--restart unless-stopped` restarts it after a reboot.

After the container starts, switch on Intel QSV hardware acceleration in Jellyfin's playback settings. The client apps are less polished than Plex's, especially on some smart TVs, and remote viewing needs a VPN or a reverse proxy.

### 6. Audiobookshelf (replaces the Audible app, \$8.99/mo)
Audible's app keeps your place across devices, so you can pick up a chapter at the same sentence. Audible Standard is \$8.99 a month. Audiobookshelf serves your own audiobooks and podcasts and syncs playback progress the same way through its phone apps.

```bash
docker run -d -p 13378:80 \
  -v /srv/audiobookshelf/config:/config -v /srv/audiobookshelf/metadata:/metadata \
  -v /srv/audiobooks:/audiobooks -v /srv/podcasts:/podcasts \
  --restart unless-stopped ghcr.io/advplyr/audiobookshelf:latest   # web UI on :13378
```

**What this does:**
- `-p 13378:80` exposes the web UI on host port 13378.
- The `/srv/audiobookshelf` mounts keep configuration and metadata in the backed-up tree.
- `/srv/audiobooks` and `/srv/podcasts` hold the files it serves.
- `--restart unless-stopped` restarts it after a reboot.

The saving is real only if you buy DRM-free audiobooks or borrow them, because Audible purchases stay locked to Audible's app.

## Home and security
Frigate feeds Home Assistant; the other two stand alone.

### 7. Vaultwarden (replaces Bitwarden Premium or 1Password, \$1.65/mo)
A password manager stores logins, fills them in on phone and browser, and warns about leaked passwords. Bitwarden Premium is \$19.80 a year, the figure in the table; 1Password at \$3.99 a month saves more. Vaultwarden is a lightweight server that speaks the Bitwarden API, so you keep the official apps and extensions and point them at your own server.

```bash
docker run -d --name vaultwarden -e DOMAIN="https://vw.example.com" \
  -v /srv/vaultwarden:/data --restart unless-stopped -p 127.0.0.1:8222:80 vaultwarden/server:latest
# after creating your account: docker rm -f vaultwarden, then re-run with -e SIGNUPS_ALLOWED=false
```

**What this does:**
- `-e DOMAIN` tells Vaultwarden the public HTTPS address it is served under.
- `-p 127.0.0.1:8222:80` binds the port to localhost only, so only a reverse proxy on the same machine can reach it.
- `-v /srv/vaultwarden:/data` keeps the vault database in the backed-up tree.
- The comment says to re-run with `SIGNUPS_ALLOWED=false` after you create your account, which closes registration.

The vault refuses to work without HTTPS, so a reverse proxy has to sit in front of it. A reverse proxy is a server that accepts requests on the public name, handles HTTPS, and forwards them to the internal app.

```javascript
vw.example.com {
    reverse_proxy 127.0.0.1:8222
}
```

**What this does:**
- The site block names the public host; Caddy obtains and renews its certificate automatically once the name points at the box and Let's Encrypt can reach ports 80 and 443.
- `reverse_proxy 127.0.0.1:8222` forwards every request to the localhost-only Vaultwarden port.
- Caddy's own certificate step means ports 80 and 443 must be reachable, which normally means forwarding them on the router; the VPN route avoids that, using the DNS challenge.
- Reload Caddy with `sudo systemctl reload caddy` after editing the file.

If Nextcloud AIO also runs, the two fight over ports 80 and 443. The article points to AIO's reverse-proxy guide, which moves AIO to `APACHE_PORT=11000` so Caddy can hold both ports.

> ❌ **Anti-pattern — leaving sign-up open:** running the password vault with registration still enabled. **Why people do it:** the default setup flow leaves it on until you create your first account. **Consequence:** anyone who can reach the server can create an account on it. **Fix:** re-run the container with `SIGNUPS_ALLOWED=false`, as the article says.

The project is unofficial and volunteer-run, and backups are your job. At \$1.65 a month the money barely matters; the point is a vault on hardware you own, at the cost of the HTTPS setup.

### 8. Pi-hole (replaces NextDNS Pro, \$1.99/mo)
Apps and web pages look up dozens of ad and tracker domains. A DNS filter refuses those lookups, so the ads never load on any device, including a smart TV that cannot run an ad blocker. NextDNS sells that as a hosted service for \$1.99 a month. The author runs Pi-hole on their own network: it answers DNS for every device with no per-device setup, and its dashboard shows which device asked for what.

```yaml
# /srv/pihole/docker-compose.yml, then: docker compose up -d
services:
  pihole:
    image: pihole/pihole:latest
    ports: ["53:53/tcp", "53:53/udp", "8081:80/tcp"]
    environment:
      TZ: "America/Los_Angeles"
      FTLCONF_webserver_api_password: "change-me"
      FTLCONF_dns_listeningMode: "ALL"
    volumes: ["./etc-pihole:/etc/pihole"]
    restart: unless-stopped
```

**What this does:**
- `ports` maps DNS on port 53 over both TCP and UDP to the host, and maps the web UI from host port 8081 to the container's port 80.
- `TZ` sets the timezone for logs and schedules.
- `FTLCONF_webserver_api_password` sets the dashboard password; `"change-me"` is a placeholder you must replace.
- `FTLCONF_dns_listeningMode` is a Pi-hole option the article does not explain, so check Pi-hole's documentation before changing it.
- The `./etc-pihole` volume keeps Pi-hole's configuration in the `/srv/pihole` folder.

On Ubuntu, systemd-resolved already holds port 53; Pi-hole's docs describe the fix. Then point the router's DNS setting at the box. The limit is that DNS blocking cannot touch ads served from the same domain as the content, which is why YouTube ads still play.

### 9. Home Assistant (replaces the per-brand smart-home apps, counted as \$0)
Each brand of bulb, plug and sensor ships its own free app, and a mixed house ends up with several apps and no way for one brand's sensor to switch another brand's light. Home Assistant puts every brand on one dashboard and runs the automations locally. Its optional Nabu Casa cloud costs \$6.50 a month if you want easy remote access, but the math counts Home Assistant as \$0.

```bash
docker run -d --name homeassistant --privileged --restart=unless-stopped \
  -e TZ=America/Los_Angeles -v /srv/homeassistant:/config \
  -v /run/dbus:/run/dbus:ro --network=host ghcr.io/home-assistant/home-assistant:stable
# web UI on :8123
```

**What this does:**
- `-v /srv/homeassistant:/config` stores the configuration in the backed-up tree.
- `-v /run/dbus:/run/dbus:ro` gives the container the host's D-Bus socket, read-only.
- `--privileged` grants broad host access; the article does not explain why it is needed, so check Home Assistant's install docs.
- `--network=host` shares the host network stack.

> 🔎 Added context: host networking lets the container reach devices on the local network directly, which device discovery usually needs.

The Docker install does not include Home Assistant's add-on store, so extras like Frigate run as separate containers you manage.

### 10. Frigate (replaces Ring Protect or Google Home Premium, \$9.99/mo)
Ring keeps no recordings without a plan, and it cannot tell a person from a passing car. Ring Protect Multi, \$9.99 a month, adds saved clips and person alerts; Google Home Premium, which replaced Nest Aware, starts at \$10. Frigate is a local NVR (network video recorder): it records your cameras and detects people, cars and animals on the box, sends no video anywhere, and feeds those events to Home Assistant.

```yaml
# /srv/frigate/docker-compose.yml, then: docker compose up -d
services:
  frigate:
    image: ghcr.io/blakeblackshear/frigate:stable
    shm_size: "512mb"
    devices: ["/dev/dri/renderD128:/dev/dri/renderD128"]   # Intel iGPU for OpenVINO
    volumes: ["./config:/config", "./storage:/media/frigate"]
    ports: ["8971:8971"]   # https://<server-ip>:8971, self-signed cert; admin password is in docker compose logs
    restart: unless-stopped
```

**What this does:**
- `shm_size: "512mb"` sets the container's shared memory; the article does not explain why Frigate needs it.
- `devices` passes the Intel render device into the container, so detection can use the iGPU.
- The `./config` and `./storage` volumes hold settings and recordings in the compose folder.
- Port 8971 serves the web UI over HTTPS with a self-signed certificate; the admin password appears in the container logs.

Passing the iGPU through is not enough on its own. Without a `detectors` block, Frigate uses the CPU detector, so add this to `./config/config.yml`:

```yaml
detectors:
  ov:
    type: openvino
    device: GPU
```

**What this does:**
- `detectors` defines the object detectors Frigate runs.
- `ov` is a name for this detector; `type: openvino` selects Intel's OpenVINO runtime.
- `device: GPU` tells OpenVINO to run on the integrated GPU rather than the CPU.
- A model block for the detector model is also needed; the OpenVINO section of Frigate's detector docs has one for each model, and each camera gets its own RTSP entry in the camera configuration docs.

> ❌ **Anti-pattern — relying on the CPU detector:** running Frigate on the default CPU detector. **Why people do it:** it works without extra configuration. **Consequence:** Frigate's docs say the CPU detector is not recommended for general use. **Fix:** use the Intel iGPU through OpenVINO, which every N100 and N150 has, or add a Coral TPU, a Hailo card or an NVIDIA GPU.

Frigate reads a local stream from each camera, usually RTSP (the standard protocol cameras use to stream video over the network). Ring cameras do not offer one. The ring-mqtt project can relay a live stream on demand, but those streams go through Ring's cloud and are not meant for continuous NVR recording. Leaving Ring means buying RTSP cameras, and that cost is not in the table.

## Writing, dev, and automation
n8n connects to anything with an HTTP API, and Immich, Paperless-ngx and Home Assistant all have one, so it can tie the earlier entries together.

### 11. LanguageTool (replaces Grammarly Pro, \$12/mo)
Grammarly Pro checks spelling, grammar and tone in email, docs and the browser, at \$12 a month billed yearly or \$30 month to month. A self-hosted LanguageTool server does the same underlining: point the browser extension or desktop app at your server and the text stays on your network.

```bash
docker run -d -p 8010:8010 --restart unless-stopped erikvl87/languagetool
```

**What this does:**
- `-p 8010:8010` exposes the server on host port 8010.
- `--restart unless-stopped` restarts it after a reboot.
- The image is a community image, which is the route the article describes as the usual one.

The self-hosted server is the basic version. LanguageTool's server docs say its AI-based rules "are only available in the cloud", so you lose the extra checks in LanguageTool Premium. The n-gram data that improves accuracy is a multi-gigabyte download on top.

### 12. n8n (replaces Zapier Professional, \$19.99/mo)
Zapier connects apps with "when this happens, do that" rules, and Professional is \$19.99 a month on an annual plan for 750 tasks. n8n is a visual workflow builder with hundreds of integrations and a code node for custom steps. Self-hosted, it has no task cap. The article's example checks Paperless-ngx's API every hour for new documents tagged "bill" and pushes a phone notification.

```bash
docker run -d --name n8n -p 5678:5678 --restart unless-stopped -v n8n_data:/home/node/.n8n n8nio/n8n
# web UI on :5678; over plain HTTP on the LAN, add -e N8N_SECURE_COOKIE=false or the login page refuses you
```

**What this does:**
- `-p 5678:5678` exposes the web UI.
- `-v n8n_data:/home/node/.n8n` keeps workflows and credentials in a named volume.
- `--restart unless-stopped` restarts it after a reboot.
- The comment's `N8N_SECURE_COOKIE=false` flag is needed only for plain-HTTP access on the LAN; over HTTPS the login cookie works as normal.

n8n is under the Sustainable Use License: free for personal and internal use, but SSO and Git sync need a paid license.

### 13. Open WebUI + Ollama (replaces ChatGPT Plus for everyday asks, \$20/mo)
ChatGPT Plus costs \$20 a month for higher limits and stronger models. If you pay mainly because you keep hitting the free cap on summaries, email rewrites and "explain this error", a local model covers those with no limit. Open WebUI is a ChatGPT-style interface and Ollama runs the models on your box, so chats and uploaded files stay local.

```bash
docker run -d -p 3000:8080 -v ollama:/root/.ollama -v open-webui:/app/backend/data \
  --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
# web UI on :3000; the first account you create becomes the admin, so sign up right away
# it starts with no models: pull a small model such as qwen3:4b, or a newer one from the Ollama library, in the admin settings
```

**What this does:**
- `-p 3000:8080` exposes the web UI on host port 3000.
- `-v ollama:/root/.ollama` stores downloaded models in a named volume.
- `-v open-webui:/app/backend/data` stores chats and accounts.
- `--restart always` restarts it after any reboot.

> 🔎 Added context: the `:ollama` tag is the variant that bundles Ollama with the interface, so one container serves both.

A mini PC limits you to small models, which are slower than ChatGPT and noticeably weaker at hard reasoning. Keep the paid plan if that is what you use it for.

> ❌ **Anti-pattern — leaving the first account unclaimed:** installing the interface and delaying the first sign-up. **Why people do it:** the first login screen looks like a step that can wait. **Consequence:** the first account created becomes the admin, so whoever reaches the port first gets that role. **Fix:** create the admin account right after install, as the article says.

### 14. Umami (replaces Plausible or PostHog, \$9/mo)
A blog or side-project site needs visitor counts, popular pages and referrers. Google Analytics is free but uses cookies, which brings the consent banner. Plausible's Starter plan is \$9 a month and Fathom starts at \$15; both are cookie-free with a one-page dashboard. Umami is the self-hosted version of that idea: one script tag on the site and a dashboard showing visitors, pages and referrers.

```bash
mkdir -p /srv/umami && cd /srv/umami
curl -O https://raw.githubusercontent.com/umami-software/umami/master/docker-compose.yml
# edit it first: set APP_SECRET and TWO_FACTOR_ENCRYPTION_KEY (openssl rand -hex 32 for each),
# change POSTGRES_PASSWORD and the matching password in DATABASE_URL,
# and change "3000:3000" to "3001:3000", since Open WebUI has 3000
docker compose up -d   # web UI on :3001; log in as admin / umami, then change the password
```

**What this does:**
- `mkdir` and `cd` put the stack under `/srv` with the rest of the backed-up data.
- `curl -O` downloads the project's compose file, which you edit before starting.
- The comments say to generate `APP_SECRET` and `TWO_FACTOR_ENCRYPTION_KEY` with `openssl rand -hex 32`, and to change the database password in two places.
- The port changes from 3000 to 3001 because Open WebUI already uses 3000.
- `docker compose up -d` starts the stack; the default login is `admin` / `umami`, which you change at once.

In the dashboard, add your site and paste the tracking script it gives you into the site's `<head>`. The downside is that you now run and back up a Postgres database.

> ❌ **Anti-pattern — keeping default credentials:** running the analytics dashboard with its default `admin` / `umami` login. **Why people do it:** the defaults are printed in the setup guide. **Consequence:** anyone who knows the defaults can read the analytics and sign in. **Fix:** change the password immediately after the first login.

### 15. mdb (replaces Obsidian Publish for reading your own notes, \$8/mo)
Syncing markdown notes is free through git, iCloud Drive or a Nextcloud folder. The paid part is reading them somewhere other than your editor: Obsidian Publish turns a notes folder into a website, at \$8 a month billed yearly or \$10 month to month. This swap fits only if you use Publish to read your own notes on other devices, not to run a public site. The author built mdb, so weigh this entry with that in mind. mdb is a web file explorer for a markdown folder, with an editor, rendered preview, Mermaid diagrams, inline review comments, and export to HTML and PDF.

```bash
bunx @sarathm09/mdb /srv/notes
```

**What this does:**
- `bunx` runs the package from Bun's package runner without a global install.
- `/srv/notes` is the folder to serve; the article suggests pointing it at the folder your notes sync to.
- The command prints `Open in browser: http://localhost:<port>`; the port is random and changes on every run.
- Bun and Node are both needed: bunx fetches mdb, but the CLI and its SQLite package run under Node.

Run it inside tmux if you want it to survive logout, since it stays in the foreground. On first run it also writes a Claude Code command file to `~/.claude/commands/mdb-review.md` and says so in the terminal.

> ❌ **Anti-pattern — an open notes server:** running mdb with no login on every network interface. **Why people do it:** it works out of the box with no configuration. **Consequence:** anyone on the network can read and edit your notes, and if Claude Code is installed, start an AI review that runs on your account. **Fix:** run it only on a LAN you trust and reach it from outside through the VPN.

## The math
The list totals \$125.57 a month, or \$1,506.84 a year at the prices above, and more wherever you pay month to month.

- **Hardware:** a Beelink EQ14 with an Intel N150, 16GB RAM and a 500GB SSD lists at \$385. Drives for photos and media are extra and not included. 16GB covers most of the list, though Immich face recognition, Frigate and a local model all busy at once is where it gets tight.
- **Power:** N100-class boxes idle around 6 W, and Frigate and Ollama raise the average. The author estimates 20 W without measuring it, which is 14.4 kWh a month, or \$2.64 at the July 2026 US residential average of 18.31 cents per kWh.
- **Payback:** \$385 against \$122.93 a month net is about three months, before drives and before any RTSP cameras that replace Ring.

Nobody pays for all fifteen. The real number is the subset you pay for today. For example, iCloud+ 2TB, Bitwarden Premium and ChatGPT Plus come to \$31.64 a month, and the box pays for itself in about 13 months after power, provided a small local model covers what you used ChatGPT for.

## Your time and backups
The table leaves out your time. You are the sysadmin now, and backups are the part you cannot skip. Start with Immich and Pi-hole, since neither needs a domain or HTTPS, and set up backups the same weekend with restic and a USB drive mounted at `/mnt/usb`.

```bash
sudo apt install restic
sudo sh -c 'umask 077; echo "pick-a-long-password" > /root/.restic-pass'   # keep a copy off this box too
sudo restic -r /mnt/usb/restic --password-file /root/.restic-pass init
```

**What this does:**
- `apt install restic` installs the backup tool.
- `umask 077` makes the password file readable only by root; the comment says to keep a copy off the box too.
- `restic ... init` creates an encrypted backup repository on the USB drive.

Save this as `/usr/local/bin/backup.sh` and make it executable with `sudo chmod +x`:

```bash
#!/bin/sh
# stop every running container so no database is mid-write, back up, start the same ones again
running=$(docker ps -q)
[ -n "$running" ] && docker stop $running
restic -r /mnt/usb/restic --password-file /root/.restic-pass backup /srv /var/lib/docker/volumes \
  --exclude /srv/media --exclude /srv/frigate/storage --exclude /var/lib/docker/volumes/ollama   # movies, footage, models: big and replaceable
[ -n "$running" ] && docker start $running
```

**What this does:**
- It records the running containers, stops them so no database is mid-write, then starts the same set again.
- The `backup` command covers `/srv` and the Docker volumes, the two paths that hold the data from earlier entries.
- The `--exclude` flags skip media, Frigate footage and Ollama models, which are large and replaceable.
- Because the script stops every container, Pi-hole's DNS drops out until the backup finishes.

Run `sudo crontab -e` and add `0 3 * * 0 /usr/local/bin/backup.sh` to run at 3 a.m. every Sunday. Run it once by hand before the first Sunday, since the first backup is the slow one, then check it with `sudo restic -r /mnt/usb/restic --password-file /root/.restic-pass snapshots`.

> ❌ **Anti-pattern — one location for backups:** keeping the backup on a drive that sits beside the server. **Why people do it:** it is the drive already plugged in. **Consequence:** a fire or theft takes both the server and its backup. **Fix:** copy the repository to another location now and then, as the article advises.

## 🧠 Mental models
### What you pay for, then the swap
**The model:** Each entry prices the paid product by what it does for you (storage, search, sharing, alerts), then matches it with the open replacement that does the same job.
**How to apply it:** Before self-hosting, list the features you actually use. If a free tier covers them, the saving is zero, as the Nextcloud entry shows.

### Count your own time
**The model:** The monthly cost table leaves out the administration time that self-hosting adds: upgrades, backups, certificates and troubleshooting.
**How to apply it:** Before you swap a service, decide who runs its upgrades and backups. Start with services that need neither a domain nor HTTPS, as the article recommends.

## 🔑 Key takeaways
- The 15 paid products total \$125.57 a month at US list prices checked on October 1, 2026, or \$1,506.84 a year.
- A mini PC with an Intel N150 and 16GB of RAM, listed at \$385, covers the whole list in the author's build; drives and cameras cost extra.
- A replacement saves money only for what you use: the Nextcloud entry saves nothing if your files fit the free tier.
- Reach the box from outside through a VPN rather than forwarded ports, and put Vaultwarden behind HTTPS.
- Frigate needs the Intel iGPU through OpenVINO, because the CPU detector is not recommended for general use.
- Set up backups first: the author's script stops every container, backs up, and restarts them, and the author advises a copy off-site.

## 🏛️ For Kenji
> 🔎 This is your interpretation, not the author's. An architect weighing SaaS against self-hosted internal tools faces the same trade: who patches the service, who holds the backups, and how much exposure the network carries. The question it raises is which of these services you would let carry data your organization is accountable for, and who would be on call when the box fails.

## 📚 Reading plan
### From the article
- [blog.s10n.dev](http://blog.s10n.dev) — the author's publication, for further posts in the same self-hosting series.
### To go further
- Immich release notes — read before every upgrade, because major versions bring breaking changes.
- Nextcloud All-in-One local-instance guide — for LAN-only access with a domain, Tailscale or the DNS challenge.
- Vaultwarden wiki, HTTPS page — for the DNS-challenge and private-certificate routes.
- Frigate detector docs, OpenVINO section — for the model block each detector needs.
- Pi-hole docs on systemd-resolved — for freeing port 53 on Ubuntu.
- Restic documentation — for repository maintenance and off-site copies.

## Concepts
- [virtualization-containers](/concepts/cloud-infra/virtualization-containers.md)
- [network-security](/concepts/security/network-security.md)
- [encryption](/concepts/security/encryption.md)
- [least-privilege](/concepts/security/least-privilege.md)
- [attack-surface](/concepts/security/attack-surface.md)
- [large-language-model](/concepts/ai-ml/large-language-model.md)
- [inference-tradeoffs](/concepts/ai-ml/inference-tradeoffs.md)
- [database-dbms](/concepts/data/database-dbms.md)
- [finops](/concepts/cloud-infra/finops.md)
- [availability-reliability](/concepts/backend-systems/availability-reliability.md)
- [infrastructure-as-code](/concepts/cloud-infra/infrastructure-as-code.md)
- [reverse-proxy](/concepts/backend-systems/reverse-proxy.md)
