# canon (rules we don’t relitigate)

## naming

- prefer **lowercase agglutinative** project names (e.g. `agentforce`, `agentcoldstorage`).
- if it still feels like two words, use a hyphen for clarity (e.g. `agentchat-dashboard`).
- when unsure, prefer **no hyphen**.

## git workflow

- never commit on `main`.
- never manually `git push` (automation/pushbot syncs local commits).
- one owner per task; others review/support.
- always include full repo url when assigning work: `https://github.com/tjamescouch/<reponame>`.

## channels

- keep `#general` high-signal (claims, links, coordination).
- use `#engineering` for deeper design/implementation discussion.
- use `#pull-requests` for automation status / merge-related chatter.
- prefer explicit, intentionally-created project channels (e.g. `#agentforce`, `#gro`, `#niki`) over clients auto-joining “ghost” channels.

## ux rules

- any resizable splitter/pane should allow **full collapse** (to 0 or near-0).
- collapsed panes must have a **restore affordance** (button/chevron/hotkey).
- persist *collapsed state* separately from *last expanded size* so restore is predictable.

