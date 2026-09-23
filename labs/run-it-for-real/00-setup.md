<!--
layout: title
eyebrow: Docker SBX · Setup
byline: About ten minutes, done once
-->

# Run it on your machine

The hands-on lab is simulated so that it works for everyone. Everything it
teaches is the real command surface — this guide installs the real CLI so you
can follow the same steps live on your own terminal.

---

<!--
layout: split
eyebrow: Pre-flight checklist
-->

# What you need before you start

<!-- region -->

### Platform support

| Platform | Status |
| -------- | ------ |
| macOS Apple Silicon | Supported |
| Linux x86_64 (Ubuntu 22.04+, KVM access) | Supported |
| Windows 11 x86_64 | Supported |
| macOS Intel | Not supported |

<!-- region -->

### Required

- A Docker account, signed in — the free tier works. Create one at
  [hub.docker.com](https://hub.docker.com/).
- An AI provider API key: Anthropic for Claude Code, OpenAI for Codex, Google
  for Gemini, or OpenCode's free tier.
- A terminal of your choice. Windows users on WSL can follow the macOS
  commands.
- macOS users need [Homebrew](https://brew.sh) — every macOS install in this
  guide uses it.

---

<!--
layout: default
eyebrow: Step 1 of 4
-->

# Install the CLI and verify it

macOS, with Homebrew:

```bash
brew install docker/tap/sbx
```

Windows (or download the MSI from the
[sbx releases page](https://github.com/docker/sbx-releases/releases)):

```bash
winget install -h Docker.sbx
```

Windows users: close and reopen your terminal after the install so `sbx` is
picked up on PATH. Then verify — this prints the installed version:

```bash
sbx version
```

---

<!--
layout: default
eyebrow: Step 2 of 4
-->

# Sign in and pick a network policy

Sign in with a **personal** Docker account. If your account belongs to an
organization with Docker AI Governance enabled, that organization's policies
override the defaults and several steps will not behave as documented.

```bash
sbx login
```

This opens a browser-based device confirmation. Afterward, if you are prompted
to pick a default network policy, choose **Balanced**. If you were not
prompted, set it explicitly:

```bash
sbx policy reset
```

```text
   1. Open         — All network traffic allowed, no restrictions.
❯  2. Balanced     — Default deny, with common dev sites allowed.
   3. Locked Down  — All network traffic blocked unless you allow it.
```

Balanced is the policy the whole workshop assumes.

---

<!--
layout: default
eyebrow: Step 3 of 4
-->

# Get an API key and store it

Create a key with your provider and name it `sbx` so you can recognize it
later. The workshop follows the Claude path; the other providers work for the
core steps.

- **Anthropic** (recommended) — [platform.claude.com](https://platform.claude.com/), API Keys, Create Key.
- **OpenAI** — [platform.openai.com/api-keys](https://platform.openai.com/api-keys).
- **Google** — [aistudio.google.com/apikey](https://aistudio.google.com/apikey); the Gemini API has a free tier.
- **OpenCode** (free) — [opencode.ai](https://opencode.ai/), Zen, API Keys.

Store it with sbx — the CLI prompts for the value, so the key never lands in
shell history, and it stays on your machine, outside the sandbox. Use your provider's name in place of `anthropic`:

```bash
sbx secret set -g anthropic
```

Verify it is stored; the value stays masked:

```bash
sbx secret ls
```

```text
SCOPE    TYPE     NAME        SECRET
global   service  anthropic   ****...****
```

---

<!--
layout: default
eyebrow: Step 4 of 4
-->

# Run the workshop for real

Create a workspace and start your first sandbox:

```bash
mkdir -p ~/workshop/sbx && cd ~/workshop/sbx
```

```bash
sbx run claude
```

The first run pulls the sandbox template image, then drops you into Claude Code
running inside a microVM. From here, follow the hands-on lab's steps in order —
the commands are the same ones you will type in your real terminal, and
`sbx ls`, `sbx policy log`, and the rest behave as the lab shows.

The CLI evolves quickly — where this guide and `sbx --help` disagree, trust
`--help`. If something misbehaves on first run, `sbx daemon restart` resolves
most issues, and the
[Get started guide](https://docs.docker.com/ai/sandboxes/get-started/) covers
platform specifics.
