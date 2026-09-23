# SBX 101 — WeAreDevelopers 2026

The SBX 101 workshop ("Hands-on with Docker Sandboxes") as a
[Simspace](https://github.com/dockersamples/simspace) site: a slide deck with
live scripted demos, the hands-on lab, a real-machine setup guide, and a
follow-along terminal page for live sessions — four cards, one deploy.

- **`labs/sbx-101-slides/`** — the opening deck (`kind: slides`). Its demo
  terminal shares the lab's simulator, so the commands on stage are the
  commands in the lab.
- **`labs/sbx-101/`** — the self-guided lab. Everything in the terminal is
  simulated in the browser; there is nothing to install and no API key is
  required. Every CLI output was captured from real `sbx` sessions; see
  [AUTHORING-NOTES.md](AUTHORING-NOTES.md).
- **`labs/run-it-for-real/`** — the real-machine setup guide: pre-flight
  checklist, install, sign-in, API key. A deck-style entry with no terminal
  pane, because its commands run on the reader's own machine.
- **`labs/live/`** — the follow-along page for live sessions: the workshop's
  commands in session order, without explanations, beside the same terminals.
  The presenter narrates; the room runs, copies, or types. Direct link:
  `…/#/labs/live/`.

This repo pins the **`docker-next`** image tag (the updated Docker brand build)
in `compose.yaml` and `.github/workflows/deploy.yml`. Move to a released tag
after the conference.

## Author locally

You only need Docker.

```bash
docker compose up dev              # live preview at http://localhost:5173
docker compose run --rm validate   # validate all labs (fails on errors)
```

Edit files under `labs/<id>/` and refresh the browser. The `labs.json` catalog
is generated — never edit or commit it.

The local preview also runs `pulse`, so presence and the instructor insights
dashboard (`#/labs/<id>/insights`, token `dev-token`) work while rehearsing.
The deployed site has tracking disabled.

To author with an agent instead: `sbx env run` (needs sbx ≥ 0.39) — see
[`.sbxenv.yaml`](.sbxenv.yaml).

## Deploy

Enable Pages (Settings → Pages → Source: "GitHub Actions"), then push to
`main`. [`deploy.yml`](.github/workflows/deploy.yml) validates the labs and
publishes; PRs are validated by [`validate.yml`](.github/workflows/validate.yml).

## Deck assets

The deck uses the docker-next theme's built-in chrome. To pin an explicit logo
instead, copy the official marks from the platform repo into
`labs/sbx-101-slides/assets/` and add `logo: assets/docker-logo-deep-blue.svg`
to the deck's `brand:` block:

```bash
mkdir -p labs/sbx-101-slides/assets
curl -fsSL -o labs/sbx-101-slides/assets/docker-logo-deep-blue.svg https://raw.githubusercontent.com/dockersamples/simspace/main/app/public/labs/tour-of-docker-slides/assets/docker-logo-deep-blue.svg
curl -fsSL -o labs/sbx-101-slides/assets/docker-logo-white.svg https://raw.githubusercontent.com/dockersamples/simspace/main/app/public/labs/tour-of-docker-slides/assets/docker-logo-white.svg
```
