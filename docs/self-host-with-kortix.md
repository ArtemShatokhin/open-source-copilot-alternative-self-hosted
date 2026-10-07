# Self-host open-source Kortix on your own box

Kortix is the open-source AI Operating System, and this guide installs it on a machine you control. It follows the official self-hosting steps at [kortix.com/docs](https://kortix.com/docs) and adds the project and session loop that turns a running stack into a company repo.

Self-hosting means the platform is yours: the agents, the skills, the company memory, the connector configuration and the triggers are files in a git repo, and the stack that runs them is one Docker Compose system on your hardware. Kortix is open source (Elastic License 2.0): self-host it, read the code and modify it.

## What you need

A Linux box with a persistent, public DNS name you can point at it. A domain gives Caddy a stable name for a TLS certificate and gives agent sandboxes a stable URL to call back to. Without a domain or a tunnel, sessions cannot run, because the sandbox has no way to reach the API.

Open ports 80 and 443. The bundled Caddy proxy uses them.

An 8 GiB host keeps the default 640 MiB memory limit on each API container. On a 16 GiB host you can raise it when API traffic reaches the default:

```bash
kortix self-host env set KORTIX_API_MEMORY_LIMIT=1024m
docker stats --no-stream
```

## Option A: one-shot bootstrap

On a bare Linux box, one command installs Docker, installs the `kortix` CLI and starts the stack:

```bash
curl -fsSL https://github.com/kortix-ai/suna/raw/dev/scripts/kortix-selfhost-up.sh \
  | bash -s -- --domain kortix.example.com --email ops@example.com
```

The script runs on Linux only. On another OS, use the manual path below.

## Option B: manual path

### 1. Install the CLI

```bash
curl -fsSL https://kortix.com/install | bash
```

The installer downloads a prebuilt binary for macOS and Linux. Windows is not supported yet. `kortix update` re-runs the install script later.

### 2. Point DNS and open the ports

Create an A or AAAA record for your domain and for `api.<domain>`, both pointing at the box's IP. Open ports 80 and 443.

### 3. Initialize the stack

```bash
kortix self-host init --domain kortix.example.com
```

This renders a `docker-compose.yml`, a `.env` file, a `Caddyfile` and `updater.sh` into `~/.config/kortix/self-host/<instance>/`. For a host with no domain, use the evaluation mode instead:

```bash
kortix self-host init --tunnel cloudflare
```

The tunnel URL changes on every restart, so use it to evaluate, not to run a team.

### 4. Start the stack

```bash
kortix self-host start
```

`kortix self-host status` shows the services, `kortix self-host logs` streams them, and `kortix self-host doctor` checks the install. The first sign-in uses the dashboard the start command reports.

### 5. Configure the sandbox provider

```bash
kortix self-host configure
```

This prompts for the sandbox provider key (Daytona is the default; Platinum and E2B are also supported) and, optionally, a managed-git token. Sign up in the dashboard, then connect your own LLM key in the model picker. Self-hosted instances use your own key by default.

## Scaffold the company and ship it

With the stack running, the CLI loop creates the repo that holds the company.

```bash
kortix init my-company
cd my-company
kortix ship
```

`kortix init` scaffolds a project directory with `kortix.yaml`, an agent in `agents/` and a skill in `skills/`. The manifest declares `kortix_version: 2` and runs the OpenCode harness.

`kortix ship` lints `kortix.yaml`, commits local changes, pushes your branch and prompts for any missing secret or connection. On its first run it also creates the cloud project and repo if you have not linked one yet. If you already cloned a repo, `kortix projects link <project-id>` writes `.kortix/link.json` and binds the folder to the project.

## Run a session and merge the first change

```bash
kortix sessions new --prompt "Build the login page" --wait
kortix sessions chat
kortix cr ls
kortix cr diff 1
kortix cr merge 1
```

Each session runs in its own sandbox, on its own branch, so the project is untouched until you merge. The agent opens a change request when its session has commits ready. `kortix cr ls` lists them, `kortix cr diff <cr>` shows the unified patch, and `kortix cr merge <cr>` lands it on the default branch. Nothing reaches the default branch without that merge.

## What runs where

Self-hosted Kortix is one generic Docker Compose system, not a family of deployment targets. The same artifact runs on a laptop, a VPS or any cloud VM.

On the box:

- Caddy, the reverse proxy that terminates TLS. Kortix renders this service only when you set `KORTIX_DOMAIN`; a domain-less instance never opens ports 80 and 443.
- `kortix-api`, `llm-gateway` and `frontend`, the three application images. They track the same channel, or a version you pin.
- The Supabase Docker distribution: Kong, GoTrue auth, PostgREST, Storage, Realtime, Studio, imgproxy, meta, functions and the Supavisor connection pooler. Kortix pins every image by digest.
- `kortix-updater`, a small container with the Docker socket mounted. It checks for a new image once a day at a fixed local time (`KORTIX_UPDATE_TIME`, default 02:00, in `KORTIX_UPDATE_TZ`, default `America/New_York`).

Data lives in two bind mounts under the instance directory: `volumes/db/data` for Postgres and `volumes/storage` for file storage. The `.env` file holds every secret and signing key.

Off the box:

- Agent sandboxes, by default at Daytona. You can configure Platinum or E2B instead. `kortix-api` reaches the sandbox provider over egress, and sandbox compute never runs on the self-host box.
- The image registry, `docker.io/kortix/*`. The updater and `kortix self-host start` pull from it, and it needs no credentials.

## What the team owns

The repo. Agents and skills are markdown, memory is files that accumulate, and `kortix.yaml` declares the machine image, the connectors each agent may reach and the triggers that start work. Every change is a diff, and any part of the company can be rolled back.

The keys. Any model, your own provider keys, chosen per agent, per session or per message.

The gate. Work lands as a change request a person reads before it merges, and connector credentials are brokered server-side so they never enter the machine.

The data. The Postgres database, the file storage and the `.env` file are on your hardware.

## Updates and backups

Every instance updates itself. Pin an exact version instead:

```bash
kortix self-host update --tag 0.9.84
```

Turn the automatic updater off with `--auto-update off`. A pinned tag wins over the channel, and each instance tracks `stable` (the default) or `latest` otherwise.

Kortix has no separate backup system. Back up all three of these before a destructive command:

- `~/.config/kortix/self-host/<instance>/volumes/db/data` (the Postgres database)
- `~/.config/kortix/self-host/<instance>/volumes/storage` (file storage)
- `~/.config/kortix/self-host/<instance>/.env` (every secret and signing key)

## Where to go next

- [Microsoft Copilot alternatives](microsoft-copilot-alternatives.md): when each open-source option fits, Kortix first.
- [Copilot licensing FAQ](copilot-licensing-faq.md): what a Microsoft Copilot licence grants, next to an open-source platform.
- [Kortix documentation](https://kortix.com/docs): the self-hosting architecture, the CLI reference and every manifest key.
- [Kortix on GitHub](https://github.com/kortix-ai/suna): the source.

Get started with open-source Kortix at [kortix.com](https://kortix.com).
