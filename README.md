# television-config

Custom channels and configuration for [television](https://github.com/alexpasmantier/television) (tv) — a blazingly fast terminal fuzzy finder.

## Install

```bash
# 1. Install tv and dependencies
brew install television bat ripgrep fd

# 2. Clone this config (backup existing config first if needed)
git clone git@github.com:fortunto2/television-config.git ~/.config/television

# 3. Install helper scripts
cp ~/.config/television/scripts/* ~/.local/bin/
chmod +x ~/.local/bin/tv-claude-*

# 4. Download standard channels (git, docker, brew, etc.)
tv update-channels

# 5. Add shell integration to .zshrc
echo 'eval "$(tv init zsh)"' >> ~/.zshrc
source ~/.zshrc
```

## Quick Reference

### Launch

| Command | What it does |
|---|---|
| `tv` | File search (default channel) |
| `tv <channel>` | Open specific channel |
| `tv list-channels` | Show all available channels |
| `Ctrl+T` | Smart autocomplete — context-aware (files, git branches, dirs) |
| `Ctrl+R` | Shell history search |

### Navigation

| Key | Action |
|---|---|
| `↑/↓` or `Ctrl+P/N` | Move through results |
| `Enter` | Confirm selection |
| `Tab` | Toggle multi-select |
| `Esc` / `Ctrl+C` | Quit |
| `Ctrl+O` | Toggle preview panel |
| `Ctrl+T` | Toggle remote control (channel switcher) |
| `Ctrl+X` | Toggle action picker |
| `Ctrl+L` | Toggle layout (landscape/portrait) |
| `Ctrl+H` | Toggle help panel |
| `Ctrl+Y` | Copy selection to clipboard |
| `Ctrl+R` | Reload source |
| `PgUp/PgDn` | Scroll preview |

### Smart Autocomplete (Ctrl+T)

Type a command prefix, then hit `Ctrl+T` — tv picks the right channel:

| You type... | Channel triggered |
|---|---|
| `cd ` | dirs |
| `cat `, `vim `, `bat ` | files |
| `git checkout ` | git-branch |
| `git add ` | git-diff |
| `claude --resume ` | claude-sessions |

## Custom Channels

### claude-sessions

Search recent [Claude Code](https://claude.com/claude-code) sessions with preview and resume.

```bash
tv claude-sessions
```

- **Preview**: shows user/AI conversation flow
- **Enter**: resumes the session via `claude --resume`
- Source: `.jsonl` transcript files from `~/.claude/projects/`

### claude-history

Search full Claude Code prompt history (all time, 6+ months).

```bash
tv claude-history
```

- Source: `~/.claude/history.jsonl` — every prompt ever sent
- Great for "what was that thing I asked about X months ago?"

### claude-memory

Full-text search across all Claude Code memory.

```bash
tv claude-memory
```

- Searches: `CLAUDE.md`, auto-memory (`~/.claude/projects/*/memory/`), `.claude/rules/`
- **Enter**: opens file in `$EDITOR` at matched line
- **Preview**: syntax-highlighted via `bat`

## Helper Scripts

Installed to `~/.local/bin/`:

| Script | Purpose |
|---|---|
| `tv-claude-sessions` | List recent sessions with IDs (from .jsonl transcripts) |
| `tv-claude-session-preview` | Render session dialog for preview panel |
| `tv-claude-history` | List full prompt history from `history.jsonl` |
| `tv-claude-memory` | Aggregate all Claude memory files via `rg` (cached 5min) |
| `tv-claude-memory-resolve` | Resolve shortened paths back to absolute |

## Adding Custom Channels

Create a `.toml` file in `cable/`:

```toml
[metadata]
name = "my-channel"
description = "What this channel does"
requirements = ["rg"]  # CLI tools needed

[source]
command = "my-source-command"
display = "{}"           # how to show each entry
output = "{}"            # what to output on selection

[preview]
command = "bat -n '{}'"  # preview command

[actions.open]
description = "Open in editor"
command = "${EDITOR:-vim} '{}'"
mode = "execute"
```

Then commit and push:

```bash
cd ~/.config/television
git add cable/my-channel.toml
git commit -m "Add my-channel"
git push
```

## Requirements

- [television](https://github.com/alexpasmantier/television) >= 0.15
- [bat](https://github.com/sharkdp/bat) — syntax-highlighted previews
- [ripgrep](https://github.com/BurntSushi/ripgrep) — content search
- [fd](https://github.com/sharkdp/fd) — file finder
- [Claude Code](https://claude.com/claude-code) — for claude-* channels
- Python 3 — for helper scripts
