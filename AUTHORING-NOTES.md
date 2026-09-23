# Authoring notes — SBX 101 (WeAreDevelopers 2026)

How this site was built, what's real, what's simulated. Companion to the
content in `labs/`. This repo is the conference edition of
[DockterDan/intro-to-sbx](https://github.com/DockterDan/intro-to-sbx), rebuilt
fresh on the multi-entry Simspace layout with the `kind: slides` deck format;
the platform-feedback history and full capture provenance live in that repo's
AUTHORING-NOTES.

## Provenance

- **The simulator** (`labs/sbx-101/simulator.yaml`) is ported intact from
  intro-to-sbx: every CLI output verified against real `sbx` v0.35.0 across
  three capture rounds on a real machine (transcripts recorded with `script`).
- **Capture round 4 (pending):** sbx has moved since (v0.38 made MCP management
  first-class and moved kits to the v2 OCI artifact format; v0.39 added
  `sbx env`). Steps 4 and 6 — text and simulated outputs — still reflect the
  v0.35 surface until round-4 transcripts land. Tracked; refresh before or
  shortly after the conference.
- **The deck** (`labs/sbx-101-slides/`) carries the SBX 101 presentation
  content; its demo terminal points at the lab's simulator
  (`simulator: ../sbx-101/simulator.yaml`), so stage demos and lab commands
  cannot drift apart. Demo slides use only commands with cold-state fallbacks
  (verified: `sbx version`, `sbx create --name sandbox-alpha …`, `sbx ls`,
  `sbx policy allow network nasa.gov`, `sbx policy ls`).

## Conference-edition decisions

- **Four catalog entries, one deploy** — deck (order 1), lab with instructions
  (order 2), real-machine setup guide (order 3), and a live follow-along page
  (order 4). The live page went through two shapes: first a chrome-less
  one-slide deck holding only the two terminals (the closest the platform
  comes to a terminal-only view — still a candidate upstream ask), then its
  final form: a lab whose sections are the workshop's commands in session
  order with no explanations, generated from the main lab's fences, so the
  room can run or copy long commands instead of transcribing them. The setup
  guide is a deck, because a `kind: lab` always renders a terminal pane and
  its commands belong on the reader's own machine. A command cheat-sheet entry
  was built and then cut to keep the landing page at four cards; the same
  content ships as the lab's Reference tab (`resources.html`).
- **`docker-next` image tag** pinned in compose + deploy per Michael's
  guidance (updated Docker brand build; not the platform default). Light mode
  is the deck default — projectors handle it better.
- **Single technical voice.** The two-track (technical/guided) explanation
  system from intro-to-sbx was dropped for this developer-conference edition.
- **Room pacing:** Introduction–Step 4 run together in the session; Steps 5–6
  are marked as self-paced. The July delivery taught us a 2-hour room needs an
  explicit core path.
- **Steps 7–8** (Docker Agent multi-agent, Model Runner) are a closing-slide
  teaser pointing at the extended workshop — not simulated, because no
  transcripts exist to ground them.
- **No tracking anywhere.** `pulse-endpoint` is left commented in deploy.yml,
  and every entry sets `tracking: false`. Verified on the docker-next build:
  the flag does NOT suppress the catalog's "Completed" chip, which renders
  from the per-browser local completion marker regardless — so returning
  finishers still see their own chips. An author-side switch for the chip
  (or honoring `tracking: false` for it) is a platform feedback item; the
  live page is a single slide, so it likely marks itself completed on first
  open.
- **Deck logo:** relies on the docker-next theme chrome; README documents how
  to pin the official SVG marks from the platform repo if wanted.

## Platform observations (this repo)

- **Catalog navigation is inconsistent between views.** The deck view offers a
  grid button back to the landing page; the lab view has a back arrow in the
  top-left instead. One affordance in one place across both views would help
  learners moving between a workshop's entries.
- **A first-class audit-log primitive would deepen realism.** The lab fakes
  `sbx policy log` with phase-gated snapshot tables that grow along the flow,
  timestamps frozen from captures. A `then.log` append effect (event, host,
  decision) plus a renderer — with a deterministic pseudo-clock — would let
  every allow/block accumulate the way the real CLI's log does, in any order
  the learner plays.
- **A compact copy-line form would serve command sheets.** The live-session
  page wants a dense list where every command has its own copy button. The
  only per-item copy affordance is a full fenced block, whose window chrome is
  heavy at 90 commands on one page; inline code is compact but has no copy
  control. A text directive like `:copyline[sbx version]` — one slim row, one
  copy button — would close the gap.
- **Embedded terminal panels want learner-side sizing.** On a terminal-only
  page, a draggable split between two panels — or a collapse control so one
  terminal can go full-height — would remove the need to author fixed pixel
  heights per layout. This lab works around it with two slides of the same
  machine (one-terminal and two-terminal layouts), at the cost of the
  transcript clearing on the flip.
- **Presenter broadcast would be a strong live-session feature.** Two tiers
  surfaced while rehearsing a guided room. Locally: the pop-out terminal
  *moves* the panel (a portal), leaving a placeholder in the main window — a
  presenter wants it rendered in both, or a read-only mirror of a terminal in
  a second same-origin window (BroadcastChannel-shaped work). Remotely:
  everything is client-side by design, so a presenter's transcript cannot
  reach attendees' browsers; determinism substitutes well (same commands, same
  outputs), but a "follow the presenter" stream — pulse has the event plumbing
  shape for it — would make guided rooms first-class.
- **Fragments cannot contain directives.** Wrapping a `:::card` in a
  `::::fragment` container (remark-directive's standard nesting, longer outer
  fence) renders nothing — the wrapped content disappears entirely rather than
  revealing on keypress. Progressive reveal therefore only works for plain
  markdown content; card-by-card builds have to be expressed as consecutive
  slides. The architecture build in this deck uses that pattern.

## Deliberate deviations from the real CLI (carried from intro-to-sbx)

- One canonical path: Claude provider, macOS-flavored host, user `me`.
- Agent replies are curated composites of real transcripts — same rendering,
  same facts, tightened for teaching.
- Setup is compressed; the API key is a fake workshop-issued string, and the
  `Enter secret:` prompt uses the engine's interactive-input primitive
  (`then.input`, masked), so any typed value is accepted — required for the
  live-session page, where there is nothing to copy from. The policy picker
  and y/N confirmations still use the older bare-token pattern with the `▊`
  convention; migrating them to `then.input` is post-conference cleanup.
- `/exit` instead of Ctrl+C twice; `sbx reset` works in-lab (state-level).
- Rule UUIDs, digests and layer hashes frozen from captures for determinism.
- The real `sbx policy log` includes a LAST SEEN timestamp column. The engine
  has no clock (deterministic by design), so rather than showing fictitious
  times, the simulated log omits that column entirely; events and counts
  accumulate along the flow.
