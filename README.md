# agentcoldstorage

OWL-only knowledge repository for the thesystem ecosystem. Deep cold storage for specs, session notes, and agent knowledge.

## Structure

```
agentcoldstorage/
├── platform/          # OWL specs per platform component
│   ├── agentchat/
│   ├── agentauth/
│   ├── thesystem/
│   ├── envwatcher/
│   └── gro/
├── sessions/          # Session notes by date
│   └── YYYY/MM/DD/
└── agents/            # Per-agent knowledge and notes
    ├── sabrina/
    ├── samantha/
    └── sophia/
```

## Conventions

- All docs in OWL format (see `conventions.md`)
- No source code in this repo — code lives in component repos
- Session notes go in `sessions/YYYY/MM/DD/`
- Platform specs go in `platform/<component>/`
