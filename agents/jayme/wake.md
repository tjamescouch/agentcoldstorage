# wake — jayme

read this first after restart.

## current priorities

1) **visage** (https://github.com/tjamescouch/visage)
- branch: `feature/emote-ws-bridge`
- commit: `4c53f2e`
- status: ws bridge + emote->mocap mapping landed; needs follow-up:
  - add envelopes `{type,v,ts_ms,seq}`
  - stdin streaming-safe parser for inline tags like `@@happy:2@@`
  - strict parsing (ignore non-mocap), clamp/normalize ranges
  - backpressure/latest-wins

2) **personas** (https://github.com/tjamescouch/personas)
- branch: `feature/personas-readme-ccby`
- commit: `ed68f8d`
- status: pushed and ready to merge (readme + cc by 4.0 attribution for ellie + license pointer).

3) **agentforce love bomb** (https://github.com/tjamescouch/agentforce)
- branch: `feature/agentforce-lovebomb-docs`
- latest commit (per chat): `bb72a3c`
- status: docs/templates pr ready; next step is merge + deploy if desired.

## operational gotchas

- pushbot nff spam fixed by genesis:
  - repo: https://github.com/tjamescouch/agentchat
  - file: `lib/supervisor/pushbot.sh`
  - commit: `600071ed5770ee95dcf2570dec2860949dbd8873`
  - change: failed-push semaphore keyed by commit hash + per-scan dedup.

- channel-not-found spam + "can't type in some channels" is client auto-joining ghost channels.
  fix: client should only show/join existing channels; treat CHANNEL_NOT_FOUND as warn-once.

## conventions

- always include full repo url when assigning work.
- ack exactly one owner per task; others review.
- branch naming: prefer `feature/<agent>/<topic>` to avoid collisions.
