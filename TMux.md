
## Layout

 **\[session\] localrun**: local stack (docker + frontend)
 - \[pane\]`docker compose up`
 - \[pane\] `npm run dev`

**\[session\] repos**: Per-repo sessions (Codex/Git/...)
- **\[window 1\] atms**
- **\[window 2\] atms-client-web**
- **\[window 3\] atms-e2e**
- **\[window 4\] atms-workspace**

**\[session\] infra** (s): VMs SSH
- **\[window 1\] staging DB**
- **\[window 2\] staging VM**
- **\[window 3\] jumpbox VM**

**\[session\] scratch**: scratch work


## Commands

Open command prompt: `Ctrl+b :`

Scroll: `Ctrl+b [`
- And then use arrows or Page Up/Down to scroll

Start a new session: `tmux new -s <name>`
Start a new session from within tmux session: `:new-session -s <name>`
Attach to a session: `tmux attach -t <name>`
Attach to a session and detach any other clients: `tmux attach -dt <name>`
Attach or start: `tmux new -As <name>`
Attach to the most recent session: `tmux attach`
Navigate between sessions: `Ctrl+b s`
List all sessions: `tmux ls`
Rename window: `Ctrl+b $`
Detach from all sessions: `Ctrl+b d`

Create a new window within the session:
- `Ctrl+b c`
- `:neww <command to be run in the new window>`
	- `-n`: window name
	- `-t`: window index
Navigate between windows:
- `Ctrl+b <i>` changes to window at index `i`
- `Ctrl+b n` changes to the next window in the window list
- `Ctrl+b p` changes to the previous window in the window list
- `Ctrl+b l` changes to the previous window
Rename window: `Ctrl+b ,`

Split window into panes:
- `Ctrl+b %`: horizontally
- `Ctrl+b "`: vertically
- `split-window <command to be run in the new pane`
	- `-h`: horizontal split
	- `-v`: vertical split
	- `-f`: new pane takes the whole width/height
	- `-b`: puts the new pane to the left or above of the pane being split instead of to the right or below
Navigate between panes: `Ctrl+b Up`, `Ctrl+b Down`, `Ctrl+b Left` and `Ctrl+b Right`

Tree view: `Ctrl+b s`
- `Enter`: switch to the selected item (session, window, pane)
- `Up`, `Down`: navigation within the tree
- `Right`: expand item
- `Left`: collapse item
- `t`: tag/untag item
- `x`: kill selected item
- `X`: kill all tagged items

Kill server and all sessions: `:kill-server`

## Useful configuration ~/.tmux.conf

Make window and pane indexing start from 1 instead of 0:
```
set -g base-index 1
set -g pane-base-index 1
set -g renumber-windows on
```

## Color themes

**Note**: TMux themes only affect tmux elements, not your whole terminal (e.g. background color). So you need to install a matching theme in both your terminal profile and tmux, e.g.:
- Terminal theme: Dracula
- tmux theme: Dracula

### Installing new theme

**Step 1** (ONLY ONCE): Clone tmux plugins:
```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

**Step 2**: Add this to `~/.tmux.conf`:
```
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'dracula/tmux' # or any other theme

run '~/.tmux/plugins/tpm/tpm'
```

**Step 3** (ONLY ONCE, OR AFTER CHANGING THEMES/PLUGINS): Install plugins:
```
Ctrl+b I
```

If you want to change the theme, you should replace `set -g @plugin 'dracula/tmux'` in your config with the corresponding theme and run `Ctrl+b I`.