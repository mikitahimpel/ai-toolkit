# ai-toolkit

Personal skills and agents for Claude Code, Codex, and OpenCode.

## Plugins

### `designer`

Opinionated design taste skill plus two subagents that apply it.

- **`designer` skill** — stack-agnostic aesthetic principles and anti-patterns. Single source of truth for what "good" looks like.
- **`design-reviewer` agent** — critiques existing UI from screenshots. Read-only; returns prioritized findings tied to principles in the skill.
- **`design-builder` agent** — implements new UI from spec. Commits to one aesthetic direction before writing code.

### `comment-discipline`

Rules for writing source-code comments that serve future readers — not the conversation that produced the code.

Prevents the common AI-coding failure where agents add comments like "switched to library X as you suggested" or "refactored from earlier approach" — comments that document the chat, not the code, and become noise to any future reader who only has the file in front of them.

## Install — user-level (global)

Use this when the plugin makes sense across all your projects.

### Claude Code

```
/plugin marketplace add mikitahimpel/ai-toolkit
/plugin install designer@ai-toolkit
/plugin install comment-discipline@ai-toolkit
```

After install, skills are available as `/designer:designer` and `/comment-discipline:comment-discipline`. Agents from the designer plugin appear in `/agents`.

### Codex

```
/plugins
```

Then add this repo as a plugin source and install the plugins. Codex installs the skill components only — agents are Claude-specific and live in `~/.codex/AGENTS.md` as a different format.

### OpenCode

If you've already installed the Claude plugin above, no separate install is needed. OpenCode scans `~/.claude/skills/` natively, so both skills are discovered automatically.

If you don't use Claude Code, drop a copy of each skill folder into `~/.config/opencode/skills/<name>/` manually.

## Install — project-level (per repo)

Use this when a specific repo should ship with these plugins so anyone who clones it gets the same setup.

### Claude Code

In the project's `.claude/settings.json` (commit this to the repo):

```json
{
  "extraKnownMarketplaces": {
    "ai-toolkit": {
      "type": "github",
      "repo": "mikitahimpel/ai-toolkit"
    }
  },
  "enabledPlugins": {
    "designer@ai-toolkit": true,
    "comment-discipline@ai-toolkit": true
  }
}
```

Anyone opening the project in Claude Code is prompted to trust the marketplace on first run, then the plugins auto-install for that project. The plugins are scoped to the project — they do not become globally enabled.

### Codex

Project-level install in Codex is configured via `.codex/config.toml` at the project root. Codex's plugin scoping is still evolving; check the Codex docs for the current syntax. The skills can also be dropped directly into the project's `.codex/skills/` directory as a no-config fallback.

### OpenCode

OpenCode reads project-level skills from `.opencode/skills/` and `.claude/skills/` at the project root. Either drop the skill folders there directly, or declare the dependency in `opencode.json`.

## Layout

```
ai-toolkit/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    ├── designer/
    │   ├── .claude-plugin/plugin.json
    │   ├── .codex-plugin/plugin.json
    │   ├── skills/designer/SKILL.md
    │   └── agents/
    │       ├── design-reviewer.md
    │       └── design-builder.md
    └── comment-discipline/
        ├── .claude-plugin/plugin.json
        ├── .codex-plugin/plugin.json
        └── skills/comment-discipline/SKILL.md
```

## License

MIT
