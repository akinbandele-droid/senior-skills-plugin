# Senior Skills — Claude Code plugin

A single installable plugin bundling **24 senior-level, cross-linked skills** that cover the
full lifecycle of designing, building, shipping, securing, and verifying software and data.

This repository is also a **plugin marketplace** (one plugin: `senior-skills`).

## What's inside (24 skills)

**Build & engineer:** `software-engineering` · `data-engineering` · `data-analyst` · `ai-engineering`
**Databases:** `database-design` · `sql-development` · `database-administration` · `database-security`
**Design:** `ui-ux-design` · `web-design` · `graphic-design`
**Ship & operate:** `devops-infra` · `cloud-security`
**Quality & docs:** `qa-testing` · `technical-writing`
**Secure, defend & govern:** `security-review` · `cyber-security-analyst` · `grc-analyst` · `incident-response` · `osint-recon` · `pentest` · `web-pentest` · `api-pentest` · `mobile-pentest`

Each skill is a lean `SKILL.md` (core workflow + principles) plus `references/*.md` loaded on
demand. They auto-trigger when a task matches, or you can invoke one with `/<skill-name>`.
See `plugins/senior-skills/README.md` for the full index and how they fit together.

> The security-testing skills (`pentest`, `web-pentest`, `api-pentest`, `mobile-pentest`,
> `osint-recon`) are **authorization-gated**: they require confirmed authorization and an agreed
> scope before any active testing.

## Install

From Claude Code, add this folder as a marketplace, then install the plugin:

```bash
/plugin marketplace add C:\Users\EMAT MSI\NexoraWork\senior-skills-plugin
```

```bash
/plugin install senior-skills@nexora-skills
```

Then restart Claude Code if prompted. The skills become available across all your projects.

To update after editing skills, bump the versions in `.claude-plugin/marketplace.json` and
`plugins/senior-skills/.claude-plugin/plugin.json`, then:

```bash
/plugin marketplace update nexora-skills
```

## Share it

Push this folder to a Git repository, then others can install with:

```bash
/plugin marketplace add <owner>/<repo>
```

```bash
/plugin install senior-skills@nexora-skills
```

## Layout

```
senior-skills-plugin/
├── .claude-plugin/
│   └── marketplace.json          # marketplace manifest (lists the plugin)
├── plugins/
│   └── senior-skills/
│       ├── .claude-plugin/
│       │   └── plugin.json        # plugin manifest
│       ├── skills/                # the 24 skill folders (SKILL.md + references/)
│       └── README.md              # full skills index & how they fit together
└── README.md                      # this file
```

## Notes

- `author` / `owner` are set to a neutral placeholder (`Nexora`) — edit them in the two manifest
  files to your name/handle before sharing.
- License is set to MIT in `plugin.json`; change if you prefer.
- The skills are a **copy** taken from `~/.claude/skills`. The originals there still work on their
  own; this plugin makes the set installable/shareable as one unit. If you keep editing the
  originals, re-copy them into `plugins/senior-skills/skills/` to refresh the plugin.
