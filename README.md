# Homelab-Umar

hi, this is my homelab. it's an old asus laptop with a broken hinge that i turned into a home server instead of throwing it away. it runs my photos, ad blocking, a dashboard and a bunch of other stuff, all in docker.

## the hardware

- asus n56vz, i5-3210m, 8gb ram
- one 750gb hdd (yes, a spinning disk, it's slow but it works)
- battery was dead so i took it out
- lid closed, no screen, i just ssh into it from my pc

## what's running

| app | what it does |
|---|---|
| immich | google photos but self hosted, my phone backs up here |
| homarr | the dashboard in the screenshot |
| adguard home | blocks ads for the whole network |
| nginx proxy manager | gives every app a nice https domain |
| uptime kuma | tells me on telegram when something goes down |
| dashdot | cpu / ram / network graphs |
| syncthing | keeps a copy of my pc files on the server |
| linkding | bookmarks |
| actual | budgeting |
| upsnap | wakes up my pc from my phone |
| dockge | manages all the compose stacks |

it runs on debian 13 with no desktop, plus docker. i tried thinking about proxmox and truenas but 8gb of ram isn't much and immich wants most of it.

for access outside home i use tailscale, so nothing is open to the internet. a wildcard cert from cloudflare means every app gets its own subdomain with https.

## want to copy it?

go for it. every app has its own folder in `stacks/` with a `compose.yaml`. you'll need to set a few things up yourself though:

1. install docker and the compose plugin
2. copy the folder of whatever app you want, e.g. `stacks/immich`
3. if there's a `.env.example`, copy it to `.env` and fill it in
   - **immich**: set `DB_PASSWORD` to something random, and change `UPLOAD_LOCATION` / `DB_DATA_LOCATION` to wherever you want your photos and database
   - **homarr**: `SECRET_ENCRYPTION_KEY` needs a random key, you can make one with `openssl rand -hex 32`
   - change `TZ` to your own timezone
4. run `docker compose up -d` in that folder
5. check the ports in the compose file, some of mine are changed so they don't clash (dashdot is on 3005 because uptime kuma took 3001)

the domains, proxy hosts and dns stuff are specific to my network so you'll have to set those up your own way.

## the not so great parts

- no backups right now, the hdd is the only copy on the server (my photos are still on my phone too)
- if the power goes out i have to turn it back on by hand, the laptop doesn't power on by itself
- it takes about 5 minutes after boot for everything to come up, old cpu + hdd
