# Wrap-up

You just ran the core arc of SBX 101, end to end:

1. **Created a sandbox** — a Linux microVM with its own kernel, a non-root `agent` user, and exactly one mounted directory.
2. **Proved the isolation** — workspace shared both ways, everything else invisible, secrets usable but unreadable. The boundary is a mount table, not a promise.
3. **Shaped the network** — Balanced policy, targeted allows, a deny that beat a default allow, and an audit log as the source of truth.
4. **Handed the agent tools over MCP** — registered servers, hit the real 409 (gateways are born at create time), recreated with `--static-mcp`, and verified at the network layer that the lookups never touched the sandbox's own network.
5. **Shipped hardened output** — the agent built the app inside the boundary; one `FROM dhi.io/...` line collapsed the CVE count.
6. **Standardized it** — a mixin kit turned your ad-hoc setup into a versionable file any teammate can attach with one flag.


## What was simulated

Every command you typed is the real `sbx` / `docker` surface, and every output matches what the real workshop shows. What the simulator changed: no installs, no API keys, no Docker daemon, and the agent's replies are scripted — deterministic on purpose, so the lab behaves identically for everyone.

## Run it on your machine

- **On your own machine:** open the **Run it on your machine** card on this site's landing page — install, sign in, store a key, and run these same steps against the real CLI.
- **Get started docs:** [docs.docker.com/ai/sandboxes](https://docs.docker.com/ai/sandboxes/get-started/).
- **The full workshop** — including the provider variants (Codex, Gemini, OpenCode), the Windows paths, the optional deep-dive exercises, and Steps 7–8 on **Docker Agent** multi-agent orchestration: [SBX 101 / AI Engineer Workshop](https://dockterdan.github.io/ai-engineer-workshop/).
- **Going deeper on kits and shared environments:** Michael Irwin's WeAreDevelopers module covers kits and `.sbxenv.yaml` end to end — [Own your dev environment](https://dockersamples.github.io/wad-2026-dev-env-workshop/).
- **Docker Hardened Images:** [docker.com/products/hardened-images](https://www.docker.com/products/hardened-images/).

One idea ties all of this together: agents are most useful when you can say **yes** to them, and you can only say yes when you know exactly what they can see, reach, and ship. That is what the sandbox is for.
