# Welcome to SBX 101

> **In the room?** The follow-along terminal for the live session is at
> [bit.ly/sbx101-cmd](https://bit.ly/sbx101-cmd).

**Docker Sandboxes (sbx)** run AI coding agents — Claude Code, Codex, Gemini, OpenCode — inside an isolated Linux microVM instead of loose on your laptop.


In this lab you will:

- Create and manage sandboxes from the CLI
- Prove the isolation boundary is real — filesystem, secrets, and all
- Control the agent's network with policy rules and an audit log
- Hand the agent tools over MCP, from registration to the gateway
- Build, scan, and harden a container image the agent writes for you
- Package the whole setup as a reusable kit

**Session pacing:** in a live session, the instructor covers the Introduction through Step 4. Steps 5 and 6 are designed to be completed at your own pace, and this site remains available after the session ends.

## How this lab works

Everything is **simulated in your browser** — nothing to install, no API keys, no network calls. Commands behave the same way every time, for everyone.

- Press **Run** on a code block to execute it, or type the commands yourself. Shell basics like `ls`, `clear`, and `cat <file>` behave the way you would expect.
- When a line ends with `▊`, the CLI is waiting for your answer — type it as your next line (the number, or `y`/`n`).
- **Terminal A** is where you talk to the agent. **Terminal B** is your host shell for watching logs while the agent works. Both are shells on the same simulated machine.
- Blocks marked **Prompt to agent** are typed *inside* an agent session, not at the shell.
- Type `/exit` to leave an agent session. (The real CLI uses Ctrl+C twice.) Inside a session, prefix a line with `!` to run a shell command without leaving — e.g. `!ls`.
- To reset the whole lab to a clean slate at any point, right-click the Docker logo in the top-left corner.

> **Note:** on a real machine you would install the CLI with `brew install docker/tap/sbx` (macOS) or `winget install -h Docker.sbx` (Windows). The simulator skips installation, but every `sbx` command here is the real command surface. The **Run it on your machine** card on the landing page covers the real setup.

## Quick setup

Verify the CLI (Terminal A):

```bash terminal-id=a
sbx version
```

Log in with your Docker account:

```bash terminal-id=a
sbx login
```

What you should see:

```output no-run-button no-copy-button
Your one-time device confirmation code is: HRLC-BPHK
Open this URL to sign in: https://login.docker.com/activate?user_code=HRLC-BPHK

Waiting for authentication...
Signed in as dockterdan.
```

> **Note:** on a fresh machine this step is optional — the first `sbx` command that needs authentication starts the sign-in flow on its own, then continues into the policy picker below.

### Pick a default network policy

Every sandbox gets a network policy. Set the default:

```bash terminal-id=a
sbx policy reset
```

The CLI restarts the daemon, resets policies, and shows the picker:

```output no-run-button no-copy-button
Initialize the global network policy for your sandboxes:

  Applies to all sandboxes, current and future — change it later with
  "sbx policy allow/deny/rm". Kits, including built-in agent kits, may
  also add per-sandbox rules.

     1. Open         — All network traffic allowed, no restrictions.
  ❯  2. Balanced     — Default deny, with common dev sites allowed.
     3. Locked Down  — All network traffic blocked unless you allow it.

  Type 1, 2, or 3 and press Enter to confirm.
```

(The real CLI also lets you drive this with ↑/↓ arrows.) Pick **Balanced** — type `2` and press Enter:

```bash terminal-id=a
2
```

What you should see:

```output no-run-button no-copy-button
Network policy set to "Balanced". Default deny, with common dev sites allowed.
```

Balanced is the policy this whole workshop assumes: everything denied by default, with allow rules for the sites developers actually need. Step 3 shows exactly what that means. One detail worth knowing from the real CLI: under Locked Down, even the agent's own API calls are blocked — the policy applies to the model's traffic like everything else.

### Store your API key

The agent needs an API key — but the sandbox should never *hold* it. `sbx` stores the key on your machine, outside the sandbox, and injects it at the network layer, so the key never enters the VM.

Run the command — it opens an `Enter secret:` prompt:

```bash terminal-id=a
sbx secret set -g anthropic
```

Here's your (workshop-issued, fake) Anthropic key. **Copy it**, paste it at the prompt, and press Enter. The input is masked as you type, and the simulator accepts any value here, so a made-up key works too:

```text no-run-button
sk-ant-api03-sbx101-demo-2f9d4e8a7c1b0356
```

Verify it's stored:

```bash terminal-id=a
sbx secret ls
```

What you should see:

```output no-run-button no-copy-button
SCOPE      TYPE      NAME        SECRET
(global)   service   anthropic   sk-ant******...******0356
```

> **In real use:** one `sbx secret set -g <provider>` per provider — `anthropic`, `openai`, `google`, or `opencode`. This lab follows the Claude path throughout.


That's it — no restarts, no environment variables. Next, create your first sandbox.
