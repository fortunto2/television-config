# television-config

Custom channels and configuration for [television](https://github.com/alexpasmantier/television) — a blazingly fast terminal fuzzy finder.

## Custom Channels

### claude-sessions

Search recent [Claude Code](https://claude.com/claude-code) sessions with preview and resume.

- **Preview**: shows user/AI conversation flow
- **Enter**: resumes the session via `claude --resume`
- Source: reads `.jsonl` transcript files from `~/.claude/projects/`

### claude-history

Search full Claude Code prompt history (all time, 6+ months).

- Source: reads `~/.claude/history.jsonl` (every prompt ever sent)
- No resume — history entries don't store session IDs

### claude-memory

Full-text search across all Claude Code memory: `CLAUDE.md` files, auto-memory, and `.claude/rules/`.

- **Enter**: opens file in `$EDITOR` at the matched line
- **Preview**: syntax-highlighted file via `bat`
- Source: aggregates files from `~/.claude/`, project roots, and auto-memory directories

## Helper Scripts

Install to `~/.local/bin/` (must be in `$PATH`):

| Script | Purpose |
|---|---|
| `tv-claude-sessions` | Lists recent sessions with session IDs |
| `tv-claude-session-preview` | Renders session transcript for preview |
| `tv-claude-history` | Lists full prompt history from `history.jsonl` |
| `tv-claude-memory` | Aggregates all Claude memory files via `rg` |
| `tv-claude-memory-resolve` | Resolves shortened display paths back to absolute |

## Setup

```bash
# Clone into television config directory
git clone git@github.com:fortunto2/television-config.git ~/.config/television

# Install helper scripts
cp scripts/* ~/.local/bin/
chmod +x ~/.local/bin/tv-claude-*

# Download standard channels
tv update-channels

# Add shell integration to your .zshrc
echo 'eval "$(tv init zsh)"' >> ~/.zshrc
```

## Requirements

- [television](https://github.com/alexpasmantier/television) >= 0.15
- [bat](https://github.com/sharkdp/bat) — for syntax-highlighted previews
- [ripgrep](https://github.com/BurntSushi/ripgrep) — for memory content search
- [Claude Code](https://claude.com/claude-code) — session data source
- Python 3 — for session/history scripts
