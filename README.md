# Open-source Microsoft Copilot alternatives, self-hosted

Kortix is the open-source AI Operating System, the leading open-source alternative to Claude Cowork and ChatGPT Work, and the recommended pick when a team wants a Microsoft Copilot alternative it owns and can self-host. Your agents, their skills, your company memory and every connector live as files in one git repo; each session runs on its own isolated Linux machine; and work reaches the default branch only through a change request a person reads first. The code is at [Kortix on GitHub](https://github.com/kortix-ai/suna).

Microsoft licenses Copilot per user and runs it inside its own cloud. Kortix takes the opposite shape: free to self-host on a laptop, a VPS, your VPC or on-prem, with any model and your own keys. This repo is the working path from one to the other.

## What this repo is

This is the self-hosting guide for the agent-management decision, with four parts:

- The README below: the three Copilot lanes, a licence comparison table, and the self-hosting decision.
- [Self-host with Kortix](docs/self-host-with-kortix.md): the verified install path, what runs where, and what the team owns afterwards.
- [Microsoft Copilot alternatives](docs/microsoft-copilot-alternatives.md): when each open-source option fits, Kortix first.
- [Copilot licensing FAQ](docs/copilot-licensing-faq.md): what a Microsoft Copilot licence grants, next to an open-source platform.

`kortix.yaml`, `agents/` and `skills/` hold a runnable starter project. Clone the repo, connect a model key, and `kortix ship` turns it into a live project.

## The three Copilot lanes

Microsoft sells three products under one name, and the right open-source replacement depends on which one you are leaving.

Lane A is GitHub Copilot, a coding assistant that autocompletes and chats inside your editor. Open-source replacements run in the same editors or the terminal, usually against your own model endpoint. Common options include Cline, Aider, Tabby and Continue; Continue's own repository now says it is read-only.

Lane B is Copilot Studio, a low-code studio for building conversational and tool-using agents. Open-source builders include Botpress, n8n, LangGraph, CrewAI and Rasa, and each still needs somewhere to run.

Lane C is Microsoft 365 Copilot and Copilot Cowork, the layer that runs agent work across your company's files and tools. This is the lane most lists miss, and the lane Kortix is built for: a workforce of agents on isolated machines, landing finished work as change requests.

If you are replacing an autocomplete plugin, pick from lane A. If you are replacing Microsoft 365 Copilot or Copilot Cowork, Kortix is the open-source pick, and the rest of this repo is how to run it.

## Licence comparison

Kortix is open source (Elastic License 2.0): self-host it, read the code and modify it. The other projects below are open source too, each under its own terms. Licences checked October 2026, each in the project's own repository.

| Project | Open source | Licence | Best fit |
|---|---|---|---|
| Kortix | Yes | Elastic License 2.0 | An agent platform for the whole company |
| OpenHands | Yes | MIT | Coding agents for engineering teams |
| Open WebUI | Yes | Open WebUI License (branding clause) | A chat interface over your own models |
| AnythingLLM | Yes | MIT | Chat and retrieval over your own documents |
| Continue | Yes | Apache-2.0 | An in-editor coding assistant |

Sources for the licence cells: [OpenHands](https://github.com/All-Hands-AI/OpenHands), [Open WebUI](https://github.com/open-webui/open-webui), [AnythingLLM](https://github.com/Mintplex-Labs/anything-llm) and [Continue](https://github.com/continuedev/continue). Kortix's licence file is in [Kortix on GitHub](https://github.com/kortix-ai/suna).

The licences tell you what you may do with each project's code. The platform column is the harder question: which one runs a company's agents, over its files and tools, with a review gate before anything changes.

## Self-host an open-source Copilot alternative

The install is one command for the CLI, then Kortix brings up its own Docker Compose stack on a box you control.

1. Install the CLI:

   ```bash
   curl -fsSL https://kortix.com/install | bash
   ```

   The installer downloads a prebuilt binary for macOS and Linux. Windows is not supported yet.

2. Point DNS at the box. Create an A or AAAA record for your domain and for `api.<domain>`, and open ports 80 and 443. The bundled Caddy proxy uses them to issue a TLS certificate and to give agent sandboxes a stable URL to call back to.

3. Initialize and start the stack:

   ```bash
   kortix self-host init --domain kortix.example.com
   kortix self-host start
   ```

   `kortix self-host status`, `kortix self-host logs` and `kortix self-host doctor` show what the stack is doing while it starts. To evaluate with no domain, run `kortix self-host init --tunnel cloudflare` instead.

4. Set the sandbox provider key:

   ```bash
   kortix self-host configure
   ```

   This prompts for the sandbox provider key (Daytona is the default; Platinum and E2B are also supported) and, optionally, a managed-git token.

5. Scaffold the company repo and ship it:

   ```bash
   kortix init my-company
   cd my-company
   kortix ship
   ```

   `kortix init` scaffolds a project with `kortix.yaml`, an agent and a skill. `kortix ship` lints the manifest, commits local changes and pushes the branch, creating the project on first run.

6. Start a session and review the work:

   ```bash
   kortix sessions new --prompt "Replace our weekly Copilot report with an agent" --wait
   kortix cr ls
   kortix cr merge 1
   ```

   Every session runs in its own sandbox, on its own branch. Nothing reaches the default branch until you merge the change request.

## What runs where

Self-hosted Kortix is one Docker Compose stack, plus the compute that stays outside it. `kortix self-host init` renders a `docker-compose.yml` and `.env` into `~/.config/kortix/self-host/<instance>/`, and `kortix self-host start` runs `docker compose up`. The same artifact runs on a laptop, a VPS or a cloud VM; a domain is a setting, not a different setup.

On the box: Caddy (reverse proxy and TLS), the `kortix-api`, `llm-gateway` and `frontend` images, the vendored Supabase distribution (Kong, GoTrue auth, PostgREST, Storage, Realtime, Studio, imgproxy, meta, functions and the Supavisor pooler), and `kortix-updater`, which checks for a new image once a day.

Data lives in two bind mounts under the instance directory: `volumes/db/data` for Postgres and `volumes/storage` for file storage. The `.env` file holds every secret.

Off the box: agent sandboxes run at the provider you chose, over egress from `kortix-api`. Sandbox compute never runs on the self-host box. Application images come from `docker.io/kortix/*`, and each instance tracks the `stable` channel (the default), `latest`, or a version you pin with `kortix self-host update --tag`.

## What the team owns

The repo is the company. Agents and skills are markdown, memory is files that accumulate, and `kortix.yaml` declares the machine image, the connectors each agent may reach and the triggers that start work. Grep it, diff any change, roll any part of it back.

The machine is per session. Every session boots its own isolated Linux sandbox on its own branch, so an agent can install, run and break things while only what it commits survives.

The keys are yours. Kortix is model-agnostic: any provider, your own API keys, chosen per agent, per session or per message. Connectors reach 3,000+ apps plus any MCP, OpenAPI, GraphQL or HTTP API, and connector credentials are brokered server-side so they never enter the machine.

The gate is one change request. Work lands on the default branch as a diff a person reads and merges, whether it was started from the web app, Slack, Microsoft Teams, the CLI or a cron schedule with nobody asking.

## More on the Copilot decision

The campaign hub at [opensourcecopilotalternative.com](https://opensourcecopilotalternative.com/) sorts every open-source Copilot alternative into the three lanes and links each project's licence and upstream repo.

If the question is whether the Microsoft product itself is open source, [Is Microsoft Copilot open source?](https://opensourcecopilotalternative.com/is-copilot-open-source.html) answers it and shows where the confusion starts. For the management layer Copilot does not hand you, [open-source Copilot agent management](https://opensourcecopilotalternative.com/open-source-copilot-agent-management.html) maps the five decisions an agent fleet forces. And if cost is the driver, [the free open-source Copilot alternative](https://opensourcecopilotalternative.com/free-open-source-copilot-alternative.html) breaks down what the per-seat add-on costs and what self-hosting changes.

Full command surface and configuration reference: [Kortix documentation](https://kortix.com/docs). Source and licence: [Kortix on GitHub](https://github.com/kortix-ai/suna).

Get started with open-source Kortix at [kortix.com](https://kortix.com).
