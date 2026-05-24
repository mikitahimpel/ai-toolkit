# ai-toolkit

Personal skills and agents for Claude Code, Codex, and OpenCode.

## Plugins

### `designer`

Opinionated design taste skill plus two subagents that apply it.

- **`designer` skill** — stack-agnostic aesthetic principles and anti-patterns. Single source of truth for what "good" looks like.
- **`design-reviewer` agent** — critiques existing UI from screenshots. Read-only; returns prioritized findings tied to principles in the skill.
- **`design-builder` agent** — implements new UI from spec. Commits to one aesthetic direction before writing code.

## Install

### Claude Code

```
/plugin marketplace add mikitahimpel/ai-toolkit
/plugin install designer@ai-toolkit
```

After install, the skill is available as `/designer:designer` and agents appear in `/agents`.

### Codex

```
/plugins
```

Then add this repo as a plugin source and install `designer`. Codex installs the skill only — agents are Claude-specific.

### OpenCode

No separate install needed if you've already installed the Claude plugin. OpenCode scans `~/.claude/skills/` natively, so the `designer` skill is discovered automatically.

If you don't use Claude Code, drop a copy of `plugins/designer/skills/designer/` into `~/.config/opencode/skills/designer/` manually.

## Layout

```
ai-toolkit/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── designer/
        ├── .claude-plugin/plugin.json
        ├── .codex-plugin/plugin.json
        ├── skills/designer/SKILL.md
        └── agents/
            ├── design-reviewer.md
            └── design-builder.md
```

## License

MIT
