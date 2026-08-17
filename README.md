# Palworld Dedicated Server

Docker Compose setup using the official `pocketpairjp/palworld-dedicated-server-docker` image,
pre-loaded with an existing world save (moved from a Windows singleplayer save into
`Saved/SaveGames/0/<world-id>/`, which is the folder dedicated servers read).

## Requirements on the VM

- Ubuntu Server (or similar Linux) inside Proxmox — NOT Docker Desktop
- Docker Engine + Docker Compose plugin installed
- UDP port 8211 forwarded from your router to this VM's IP

## Setup

```bash
git clone <your-repo-url>
cd palworld-server
docker compose up -d
```

First boot will take a few minutes (downloads the image, generates default config).

## Set max players (4-6)

The default config only exists after the first boot. After the container has started once:

```bash
docker compose down
```

Edit `Saved/Config/LinuxServer/PalWorldSettings.ini`, find the `OptionSettings=(...)` line,
and set `ServerPlayerMaxNum=6` (or your preferred number) inside it. Adjust `ServerName`,
`ServerPassword`, `Difficulty`, etc. in the same line if wanted.

```bash
docker compose up -d
```

## Operating

- View logs: `docker compose logs -f`
- Stop: `docker compose down`
- Update image: bump the tag in `compose.yaml`, then `docker compose up -d`

## Notes

- The `Saved/` folder is the single source of truth for the world — back it up regularly
  (`Saved/SaveGames/0/<world-id>/backup/` already has Palworld's own auto-backups).
- This repo commits actual save-game binary data. Fine for a one-time transfer, but git
  is not a good fit for ongoing save syncing (binary diffs bloat the repo over time) —
  use `rsync`/`scp` for regular backups instead of committing new saves repeatedly.
