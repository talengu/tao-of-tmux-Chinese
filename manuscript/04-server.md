# 服务（Server ）{#server} 

服务上面建立住 [sessions](05-session.md) 、[windows](06-window.md) 、[panes](07-pane.md) 。

开启 tmux，通过socket机制连接一个 tmux 服务（server）。与你交互的其实是tmux的client客户端。本节我们将到启用这个server引擎，应用程序可以一次持续数月甚至数年。

![](images/info/server.png)

## What? tmux is a server?（什么？tmux 是一个服务器？）

通常，当提到"服务器"时，许多人可能会想到机架式硬件；对其他人来说，它可能是在服务器上以守护进程方式运行的软件，通过 upstart、supervisor 等工具来管理。

与 web 或数据库软件不同，tmux 不需要专门的配置设置或创建服务条目来启动。

tmux 使用客户端-服务器模型，但服务器会为你 fork 到后台运行。

## Zero config needed（无需配置）

你没有注意到，但当你正常使用 tmux 时，服务器会被启动并通过客户端连接。

tmux 如此流畅，本书可以继续解释用法甚至不提服务器。但是，我更希望你对它在系统上的工作方式有真正的理解。实现感觉像魔法，同时符合 unix 的功利主义期望。从用户体验的角度来看，不能否认它的执行非常巧妙。

它是如何功利的？我们将在未来的章节中深入探讨 [Formats](#formats)、[Targets](#targets)，以及我制作的 [libtmux](https://github.com/tony/libtmux) 等工具，它们利用了这些特性。

这让一些人惊讶，因为服务器通常会产生设置过程。但涉及的服务器并不需要在每台运行的机器上进行数小时的配置。没有设置。

当人们想到服务器时，他们想到的是痛苦。它唤起了在 `/etc/` 中挖掘配置文件并打开关闭设置以使基本系统上线的画面。但 tmux 不是这样。它是一个服务器，但是是好的那种。

## Stayin' alive（保持活力）

tmux 的服务器部分是你的会话即使在客户端脱离后也能保持活力的方式。

你可以从 SSH 服务器上脱离 tmux 会话，然后再连接。
你可以脱离 tmux 会话，停止 Linux/BSD 上的 X 服务器，然后在 TTY 或新的 X 服务器上重新附加你的 tmux 会话。

tmux 服务器不会消失，直到所有会话都关闭。

## Servers 包含 sessions（服务器包含会话）

一个 server 包含一个或多个 [sessions](05-session.md)。

在服务器已经运行后启动 tmux 将在现有服务器内创建一个新会话。

```txt
W> ### Advanced: Multiple servers
W>
W> tmux 是敏捷的。要使用单独的服务器，请在任何命令上传递 `-L` 标志。
W>
W> `tmux -L moo` - 连接到名为 "moo" 的 socket 服务器并附加一个新会话。如果 socket 不存在则创建服务器。
W>
W> `tmux -L moo attach` 将尝试重新附加一个会话（如果存在的话）。
```
## How servers are "named"（服务器如何"命名"）

服务器的默认名称是 `default`，存储在 `/tmp` 中的 socket 中。可以通过设置 `TMUX_TMPDIR` 环境变量来覆盖默认目录。

所以，类似这样：

```
    $ export TMUX_TMPDIR=$HOME
    $ tmux
```
将会在你的 `$HOME` 文件夹中创建一个 tmux 目录。在 OS X 上，你的 home 文件夹可能是类似 `/Users/yourusername` 的路径。在其他系统上，可能是 `/home/yourusername`。如果你想知道，输入 `$ echo $HOME`。

## Clients（客户端）

服务器会有客户端（你）连接到它们。

当你连接到一个会话并看到窗口和窗格时，这就是一个到 tmux 的客户端连接。

你可以获取活动客户端连接的列表：

```
    $ tmux list-clients
```
这些命令和其他 `list-` 命令在实践中很少见。但是，如果你想发挥创意，它们是 tmux 脚本化的一部分。[脚本化 tmux](#scripting-tmux) 章节将更详细地介绍这一点。

## Clipboard（剪贴板）{#clipboard}

tmux 客户端拥有一个共享剪贴板的特性，在 sessions、windows 和 panes 中都很有用。

与 vi 类似，tmux 将复制处理为一个模式，窗格会临时进入该模式。在这个模式下，可以选择文本并复制到*粘贴缓冲区*，即 tmux 的剪贴板。

进入复制模式的默认键是 `Prefix` + `[`。

1. 在其中，使用 `[space]` 进入复制模式。
2. 使用方向键调整要选择的文本。
3. 按 `[enter]` 复制选定的文本。

粘贴复制文本的默认键是 `Prefix` + `]`。

> *类似 Vi 的复制粘贴*
>
> 在你的[配置](#config)中，添加这个：
>
> ```
>     # Vi copypaste mode
>     set-window-option -g mode-keys vi
>     bind-key -t vi-copy 'v' begin-selection
>     bind-key -t vi-copy 'y' copy-selection
> ```

除了"复制模式"，tmux 还有高级功能可以编程方式复制和粘贴。在本书后面的章节中，[脚本化 tmux](#scripting-tmux) 章节中的[捕获窗格内容](#capture-pane)部分将介绍 `$ tmux capture-pane` 以及如何使用 [targets](#targets) 将窗格内容复制到粘贴缓冲区或使用 `$ tmux save-buffer` 保存到文件。

## 小节

服务器是 tmux 的基本基础之一。它对用户自动初始化，通过 fork 到后台来保持持久化。在后台运行，它确保会话、窗口、窗格和缓冲区即使在客户端脱离时也能运行。

服务器可以容纳一个或多个*会话*。你可以通过剪贴板在会话之间复制粘贴。在下一章中，我们将更深入地探讨会话的角色以及它们如何帮助你组织和控制终端工作区。
