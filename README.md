# codex-recent

An `fzf` picker for finding and resuming recent [Codex CLI](https://developers.openai.com/codex/cli/) conversations across working directories.

It reads Codex's local SQLite state in read-only mode, orders conversations by their last activity, and resumes the selected session in its original directory.

## Features

- Fuzzy selection across recent conversations
- Relative and exact last-used timestamps
- Vim-style `j` / `k` navigation (arrow keys also work)
- Conversation details in a preview pane
- Resume from the conversation's original working directory
- Filters by count, age, and session source
- Plain-text output for scripts

## Requirements

- macOS or Linux
- Bash 3.2+
- [Codex CLI](https://developers.openai.com/codex/cli/)
- [`fzf`](https://github.com/junegunn/fzf)
- `sqlite3`

## Install

```sh
git clone https://github.com/beejmaxx/codex-recent.git
cd codex-recent
install -m 755 codex-recent ~/.local/bin/codex-recent
```

Make sure `~/.local/bin` is on your `PATH`.

## Usage

```text
codex-recent [COUNT] [--hours HOURS] [--list] [--all-sources]
```

```sh
codex-recent                 # pick from the latest 20 conversations
codex-recent 50              # pick from the latest 50
codex-recent --hours 24      # only conversations active in the last day
codex-recent --all-sources   # include subagent/non-interactive sessions
codex-recent --list          # print instead of opening fzf
```

Use arrow keys or `j` / `k` to move, Enter to resume, and Esc to cancel. Since plain `j` and `k` are navigation keys, use another part of the title or path when fuzzy-searching.

## How it works

`codex-recent` locates the newest `state_*.sqlite` database under `${CODEX_HOME:-~/.codex}`, queries its `threads` table without modifying it, and runs:

```sh
codex resume SESSION_ID
```

from the selected conversation's saved working directory.

## License

[MIT](LICENSE)
