# Open-source Microsoft Copilot alternatives, and when each fits

Kortix is the open-source AI Operating System and the first pick for replacing the Microsoft Copilot layer that runs agent work across a company. The agents, skills, company memory and connectors live as files in one git repo you own, every session runs on its own isolated Linux machine, and work lands as a change request a person reads first. The other open-source projects below are strong in narrower lanes, and this guide says which lane each one serves.

Microsoft brands three products as Copilot: GitHub Copilot for code, Copilot Studio for building agents, and Microsoft 365 Copilot with Copilot Cowork for running agents over your files and tools. Naming the lane first matters, because an autocomplete plugin and a company-wide agent layer are different purchases.

## Kortix

Kortix is a company platform, and the whole company is one git repo. Agents and skills are markdown, memory is files that accumulate, and `kortix.yaml` declares the machine image, the connectors each agent may reach and the triggers that start work. You can grep the entire configuration, diff any change and roll any part of it back.

Six properties decide the Copilot comparison:

- One git repo is the company. Not settings in a vendor's database; files you own.
- Every tool the company runs on. 3,000+ apps in a click, plus MCP, OpenAPI, GraphQL and raw HTTP. Connector credentials are brokered server-side and never enter the machine, and each tool call can be set to allow, ask or block, down to the arguments of a single shell command.
- Any model, your keys. Claude, OpenAI, Gemini or your own OpenAI-compatible endpoint, per agent, per session or per message.
- A real agent harness, powered by OpenCode: planning, tool use and multi-step runs that finish, with permissions per tool.
- An isolated Linux machine per session. Thousands run in parallel, each on its own branch, and the agent can install, run and break things because only commits survive.
- One gate to land work. Start agents from the web app, Slack, Microsoft Teams, email, mobile, CLI or API, or from cron and webhooks with nobody asking, and the work arrives as a change request a human reads as a diff.

Self-hosting is one Docker Compose stack on a laptop, a VPS, your VPC or on-prem, installed with `curl -fsSL https://kortix.com/install | bash` and brought up with `kortix self-host start` (the full path is in [Self-host with Kortix](self-host-with-kortix.md)). Managed cloud is the alternative when nobody wants to own the machine. Kortix is open source (Elastic License 2.0): self-host it, read the code and modify it.

Pick Kortix when the job is running a workforce of agents over company files and tools, with the configuration in git and a person approving what changes.

## OpenHands

OpenHands describes itself as a self-hosted developer control center for coding agents and automations. It runs OpenHands, Claude Code, Codex, Gemini or any Agent-Client Protocol-compatible agent across local, remote and cloud backends, and it can automate tasks such as generating reports to Slack or decomposing GitHub issues. The MIT licence is in its repository.

Pick OpenHands when the job is coding agents for an engineering team and you want a control center around them. It does not try to be the company platform: Kortix covers the same agent work plus Slack and Teams sessions, cron schedules, 3,000+ connectors and a change-request gate, with the same isolated-sandbox-per-session model.

## Open WebUI

Open WebUI is a self-hosted AI platform and interface for local and cloud models, supporting Ollama and OpenAI-compatible APIs, with RBAC, plugins, agents, persistent memory and local RAG. Its licence is the Open WebUI License, a BSD-3-style text with a branding clause above fifty end users and an enterprise licence path.

Pick Open WebUI when the team wants a chat window over its own models and documents. It is a chat-first surface; it does not run a company's agent workforce over files and tools with a review gate, which is the Copilot Cowork question.

## AnythingLLM

AnythingLLM is an all-in-one AI application for chatting with your documents, with built-in agents, multi-user support and document pipelines, running locally by default. The MIT licence is in its repository.

Pick AnythingLLM when the goal is a private document-chat deployment with agent features on top. AnythingLLM is built for chat and retrieval; the agent-fleet job belongs to Kortix.

## Continue

Continue is a coding agent available as a CLI, a VS Code extension and a JetBrains plugin, under Apache-2.0. Its repository states the project is no longer actively maintained and is read-only, with a final 2.0.0 release of all three surfaces.

Pick Continue when you want an in-editor assistant you can fork and maintain yourself, and you accept the maintenance status above. The alternatives hub lists Cline, Aider and Tabby alongside it.

## How to choose

Start with the lane, then the licence, then the platform question.

| You need | Open-source pick | Why |
|---|---|---|
| Agents that run the company over files and tools | Kortix | One repo you own, isolated machine per session, change-request gate |
| A coding-agent control center for engineers | OpenHands | MIT, self-hosted, multi-backend |
| A chat interface over your own models and docs | Open WebUI | Self-hosted, provider-agnostic, RBAC |
| Private document chat with agents on top | AnythingLLM | MIT, local-first, multi-user |
| An in-editor assistant you can fork | Continue | Apache-2.0, CLI and IDE plugins; repo is read-only |

Kortix is the recommendation for the agent-management lane, and the only one of the five built to run a company rather than one kind of work.

## Where Microsoft Copilot still fits

If the team lives inside Word, Excel, Outlook and Teams and wants AI already embedded there with nothing to host, Microsoft Copilot does that job, and the per-seat licence buys the integration. Kortix answers a different question: agents that do real work on real machines, on hardware you choose, with the configuration in git.

For the licensing details behind that trade, read the [Copilot licensing FAQ](copilot-licensing-faq.md), and for the install path read [Self-host with Kortix](self-host-with-kortix.md). The alternatives hub at [opensourcecopilotalternative.com](https://opensourcecopilotalternative.com/) keeps the three-lane map current. The full command surface and manifest reference are in the [Kortix documentation](https://kortix.com/docs), and the source is at [Kortix on GitHub](https://github.com/kortix-ai/suna).

Get started with open-source Kortix at [kortix.com](https://kortix.com).
