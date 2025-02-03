# GitHub CLI extension TUI for viewing your notifications

[fzf](https://github.com/junegunn/fzf#installation)-built [GitHub CLI](https://github.com/cli/cli) extension [TUI](https://en.wikipedia.org/wiki/Text-based_user_interface) to view [your GitHub notifications](https://github.com/notifications) from your terminal — _and mark them as done_.

> [!NOTE]
> This is a [meiji163/gh-notify](https://github.com/meiji163/gh-notify) fork that adds the capability to mark notifications as done.

https://github.com/meiji163/gh-notify/assets/92653266/b7d7fcdb-8a25-43fc-8f63-d11f30960084

## Install

Make sure you have the [GitHub CLI](https://github.com/cli/cli#installation) (the `gh` command) and [fzf](https://github.com/junegunn/fzf#installation) installed. Then:

```sh
# install
gh ext install sideshowbarker/gh-notify
# upgrade
gh ext upgrade sideshowbarker/gh-notify
# uninstall
gh ext remove sideshowbarker/gh-notify
```

> [!IMPORTANT]
> To actually be able to use `gh notify` as an interactive [TUI](https://en.wikipedia.org/wiki/Text-based_user_interface), you _must_ install [fzf](https://github.com/junegunn/fzf#installation).

## Usage

```
gh notify [options]
```

| Options | Description                                             | Example                                              |
| :------ | :------------------------------------------------------ | :--------------------------------------------------- |
|  _none_ | show all unread notifications                           | `gh notify`                                            |
| `-a`      | show all (read/ unread) notifications                   | `gh notify -a`                                         |
| `-e`      | exclude notifications matching a string (REGEX support) | `gh notify -e "MyJob"`                                 |
| `-f`      | filter notifications matching a string (REGEX support)  | `gh notify -f "Repo"`                                  |
| `-h`      | show the help page                                      | `gh notify -h`                                         |
| `‑n NUM`  | max number of notifications to show                     | `gh notify -an 10`                                     |
| `-p`      | show only participating or mentioned notifications      | `gh notify -ap`                                        |
| `-r`      | mark all notifications as read                          | `gh notify -r`                                         |
| `-s`      | print a static display                                  | `gh notify -an 10 -s`                                  |
| `‑u URL`  | (un)subscribe a URL, useful for issues/PRs of interest  | `gh notify -u <url>`                                   |
| `-w`      | display the preview window at startup                   | `gh notify -an 10 -w`                                  |

### Key bindings fzf

| Key       | Description                                                       | Customization environment variable |
| :-------- | :--------------------------------------------------               | :--------------------------------- |
| `?`         | toggle help                                                       | `GH_NOTIFY_TOGGLE_HELP_KEY`          |
| `↑↓`        | move the pointer up/down                                          |                                    |
| `ctrl-j/k`  | move the pointer up/down                                          |                                    |
| `ctrl-n/p`  | move the pointer up/down                                          |                                    |
| `enter`     | show issue/PR for notification at pointer                         | `GH_NOTIFY_VIEW_KEY`                 |
| `tab`       | show/hide preview for notification at pointer                     | `GH_NOTIFY_TOGGLE_PREVIEW_KEY`       |
| `shift‑tab` | resize the preview window                                         | `GH_NOTIFY_RESIZE_PREVIEW_KEY`       |
| `shift‑↑↓`  | scroll the preview-window contents up/down                        |                                    |
| `ctrl-a`    | mark all displayed notifications as read, and reload                | `GH_NOTIFY_MARK_ALL_READ_KEY`        |
| `ctrl-b`    | open in browser                                                   | `GH_NOTIFY_OPEN_BROWSER_KEY`         |
| `ctrl-d`    | view diff                                                         | `GH_NOTIFY_VIEW_DIFF_KEY`            |
| `ctrl-f`    | view diff in patch format                                         | `GH_NOTIFY_VIEW_PATCH_KEY`           |
| `ctrl-r`    | reload                                                            | `GH_NOTIFY_RELOAD_KEY`               |
| `ctrl-t`    | mark selected notification as read, and reload                | `GH_NOTIFY_MARK_READ_KEY`            |
| `ctrl-w`    | mark selected notification as read+done, and reload       | `GH_NOTIFY_MARK_READ_KEY`            |
| `ctrl-x`    | write a comment with the editor and quit                          | `GH_NOTIFY_COMMENT_KEY`              |
| `ctrl-y`    | toggle the selected notification                                  | `GH_NOTIFY_TOGGLE_KEY`               |
| `esc`       | quit                                                              |                                    |

### Notifications-list format

![image](https://github.com/user-attachments/assets/f54363a3-9e19-4ab9-b755-bec448e7812a)

| Column                        | Description                                                  |
| ----------------------        | ------------------------------------------------------------ |
| `▶` (red)                       | pointer                                                      |
| `•` (magenta)                   | indicates unread status                                      |
| `0h ago` / `01 Foo 00:00`         | time of last read for unread; otherwise, time of last update |
| `foo/bar`                       | repository that generated the notification                   |
| `Issue`/`PullRequest`/`Discussion`  | issue/PR/discussion number                                   |
| `#0000`                         | associated number                                            |
| `subscribed`, etc.              | trigger reason                                               |
| `Foo the bar baz`               | issue/PR/discussion title                                    |

---

## Customizations

### fzf

You can customize the `fzf` key bindings by exporting particular environment variables to your environment. For available keys and events, see `man fzf` or the [Environment-variables](https://github.com/junegunn/fzf#environment-variables) section of the `fzf` online docs.

> [!NOTE]
> [How to use ALT commands in a terminal on macOS?](https://superuser.com/questions/496090/how-to-use-alt-commands-in-a-terminal-on-os-x)

```sh
# ~/.bashrc or ~/.zshrc
# The examples below enable you to clear the input query with alt+c,
# jump to the first/last result with alt+u/d, refresh the preview window with alt+r
# and scroll the preview in larger steps with ctrl+w/s.
export FZF_DEFAULT_OPTS="
--bind 'alt-c:clear-query'
--bind 'alt-u:first,alt-d:last'
--bind 'alt-r:refresh-preview'
--bind 'ctrl-w:preview-half-page-up,ctrl-s:preview-half-page-down'"
```

#### GH_NOTIFY_FZF_OPTS

This environment variable lets you specify additional options and key bindings to customize the search and display of notifications. Unlike `FZF_DEFAULT_OPTS`, `GH_NOTIFY_FZF_OPTS` specifically applies to the `gh notify` extension.

```sh
# --exact: Enables exact matching instead of fuzzy matching.
GH_NOTIFY_FZF_OPTS="--exact" gh notify -an 5
```

```sh
# With the height flag and ~, fzf adjusts its height based on input size without filling the entire screen.
# Requires fzf +0.34.0
GH_NOTIFY_FZF_OPTS="--height=~100%" gh notify -an 5
```

#### Modify key bindings

You can also customize the key bindings to your liking. For example, to make `ctrl-o` be the key binding for opening a browser on the notification currently pointed to, you can do this:

```sh
GH_NOTIFY_OPEN_BROWSER_KEY="ctrl-o" gh notify
```

> [!NOTE]
> Any key you assign a binding for must be a key listed as valid in the `fzf` man page:
>
> ```sh
> man --pager='less -p "AVAILABLE KEYS"' fzf
> ```

### Specify preferred editor

In the `gh` tool's config file, you can specify your preferred editor. This is particularly useful when you use the `ctrl-x` hotkey to comment on a notification.

```sh
# To see more details
gh config
# For example, you can set the editor to Visual Studio Code or Vim.
gh config set editor "code --wait"
gh config set editor vim
```
