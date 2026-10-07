# Open-source Copilot licensing FAQ

Kortix is the open-source AI Operating System, and this FAQ answers the licensing questions teams ask when they compare it with Microsoft Copilot. Each answer links its primary source.

## Is Microsoft Copilot open source?

No. Microsoft Copilot is a proprietary cloud service that runs inside Microsoft's commercial cloud, and Microsoft licenses it per user rather than shipping code you can audit. One part is open: GitHub publishes the Copilot Chat extension for Visual Studio Code under the MIT licence, and that client-side editor front end is where much of the confusion starts. The service, its orchestration and the models stay proprietary. Sources: [GitHub Copilot Chat licence](https://github.com/microsoft/vscode-copilot-chat) and [Microsoft Copilot plans](https://www.microsoft.com/en-us/copilot/pricing/business).

## Can you self-host Microsoft Copilot?

No. Microsoft runs Copilot as a shared service inside the Microsoft 365 boundary, and you cannot download it, deploy it in your own VPC or point it at a model endpoint you supply. Kortix is the opposite: the platform is one Docker Compose stack you run on a laptop, a VPS, your VPC or on-prem, and the install path is public. Source: [Kortix self-hosting docs](https://kortix.com/docs/host).

## What does a Microsoft 365 Copilot licence include?

A Microsoft 365 Copilot licence is an add-on that requires a separate qualifying Microsoft 365 plan for every user. Copilot Chat is included at no additional cost with eligible Microsoft 365 plans; the licensed product adds Copilot inside Word, Excel, PowerPoint, Outlook and Teams, plus Microsoft's prebuilt agents. The configuration and the accumulated knowledge stay in Microsoft's tenant, and Microsoft states that canceling a subscription deletes its associated data. Source: [Microsoft Copilot plans](https://www.microsoft.com/en-us/copilot/pricing/business).

## How is Microsoft Copilot Studio licensed?

Copilot Studio bills by consumption, per tenant, rather than per seat. Agents consume Copilot Credits, sold through a Copilot Studio subscription: one credit pack is 25,000 Copilot Credits per month at $200 per pack per month, billed annually, and unused credits do not roll over. A pay-as-you-go meter bills at $0.01 per Copilot Credit with no up-front commitment. Building and managing agents requires a Copilot Studio User License, listed at $0. Source: [Microsoft Copilot Studio licensing guidance](https://www.microsoft.com/licensing/guidance/microsoft-copilot-studio).

## What licence is Kortix under?

Kortix is open source (Elastic License 2.0): self-host it, read the code and modify it. The licence covers the platform, including the harness and the control plane. The licence text is in [Kortix on GitHub](https://github.com/kortix-ai/suna).

## Which open-source Copilot alternatives exist, and under what licences?

By lane:

- Agent management: Kortix, open source (Elastic License 2.0).
- Coding agents: OpenHands (MIT), Continue (Apache-2.0).
- Chat and retrieval: Open WebUI (Open WebUI License, a BSD-3-style text with a branding clause), AnythingLLM (MIT).

Licences checked October 2026, each in the project's own repository: [OpenHands](https://github.com/All-Hands-AI/OpenHands), [Open WebUI](https://github.com/open-webui/open-webui), [AnythingLLM](https://github.com/Mintplex-Labs/anything-llm), [Continue](https://github.com/continuedev/continue).

## Does open source mean free?

The software is free to self-host; the compute and the model tokens are not. Kortix is free to run on your own hardware, and you connect your own model keys, so you pay your provider for tokens and your box for compute. On managed cloud, the Free plan is $0 with 200 sandbox credits each month and one project, and Team is $40 per seat per month with 2,500 pooled credits per seat. Source: [Kortix pricing](https://kortix.com/pricing).

## What does an open-source licence let a team do that a seat does not?

It lets the team read and modify the platform, host it where the data has to live, and keep the agent definitions under version control. In Kortix, agents, skills, company memory, connector configuration and triggers are files in one git repo, so any change is a diff and any part of the company can be rolled back. Source: [Kortix documentation](https://kortix.com/docs).

## Where to go next

- [Self-host with Kortix](self-host-with-kortix.md): the verified install path.
- [Microsoft Copilot alternatives](microsoft-copilot-alternatives.md): when each open-source option fits.
- The campaign hub at [opensourcecopilotalternative.com](https://opensourcecopilotalternative.com/) covers the licence question across every lane.

Get started with open-source Kortix at [kortix.com](https://kortix.com).
