# 服务（Server ）{#server} 

服务上面建立住 [sessions](05-session.md) 、[windows](06-window.md) 、[panes](07-pane.md) 。

开启 tmux，通过socket机制连接一个 tmux 服务（server）。与你交互的其实是tmux的client客户端。本节我们将到启用这个server引擎，应用程序可以一次持续数月甚至数年。

![](images/info/server.png)

## What? tmux is a server?

Often, when "server" is mentioned, what comes to mind for many
may be rackmounted hardware; to others, it may be software running
daemonized on a server and managed through a utility, like upstart,
supervisor, and so on.

Unlike web or database software, tmux doesn't require specialized
configuration settings or creating a service entry to start things.

tmux uses a client-server model, but the server is forked to the 
background for you.

## Zero config needed

You don't notice it, but when you use tmux normally, a server is launched and being connected via a client.

tmux is so streamlined, the book could continue to explain usage and not even
mention servers. But, I'd rather you have a true understanding of how it works
on systems. The implementation feels like magic, while living up to the unix
expectations of utilitarianism. One cannot deny it's exquisitely executed
from a user experience standpoint.

How is it utilitarian? We'll go into it more in future chapters, where we dive
into [Formats](#formats), [Targets](#targets), and tools, such as [libtmux](https://github.com/tony/libtmux) I made, which utilize these features.

It surprises some, because servers often beget a setup process. But servers
being involved doesn't entail hours of configuration on each machine you run on.
There's no setup.

When people think server, they think pain. It invokes an image of digging
around `/etc/` for configuration files and flipping settings on and off just to
get basic systems online. But not with tmux. It's a server, but in the good way.

## Stayin' alive

The server part of tmux is how your sessions can stay alive, even after your client
is detached.

You can detach a tmux session from an SSH server and reconnect later.
You can detach a tmux session, stop your X server in Linux/BSD, and reattach
your tmux session in a TTY or new X server.

The tmux server won't go away until all sessions are closed.

## Servers 包含 sessions

一个 server包含一个或多个 [sessions](05-session.md).

Starting tmux after a server already is running will create a new session inside
the existing server. 

```txt
W> ### Advanced: Multiple servers
W>
W> tmux is nimble. To use a separate server, pass in the `-L` flag to any
W> command.
W>
W> `tmux -L moo` - connect to server under socket name "moo" and attach
W> a new session. Create server if none already exists for socket.
W>
W> `tmux -L moo attach` will attempt to re-attach a session if one exists.
```
## How servers are "named"

The default name for the server is `default`, which is stored as a socket in
`/tmp`. The default directory for storing this can be overridden via setting
the `TMUX_TMPDIR` environment variable.

So, something like:

```
    $ export TMUX_TMPDIR=$HOME
    $ tmux
```
Will give you a tmux directory created within your `$HOME` folder. On OS X,
your home folder will probably be something like `/Users/yourusername`. On
other systems, it may be `/home/yourusername`. If you want to find out, type
`$ echo $HOME`.

## Clients

Servers will have clients (you) connecting to them.

When you connect to a session and see windows and panes, it's a client
connection into tmux.

You can retrieve a list of active client connections via:

```
    $ tmux list-clients
```
These commands and the other `list-` commands, in practice, are rare. But, they
are part of tmux scriptability should you want to get creative. The [scripting tmux](#scripting-tmux)
chapter will cover this in greater detail.

## Clipboard {#clipboard}

tmux clients 拥有一个共享clipboard的特性，在essions, windows, 和 panes 都有用。



Much like vi, tmux handles copying as a mode in which a pane is
temporarily placed. When inside this mode, text can be selected and copied to
the *paste buffer*, tmux's clipboard.

The default key to enter copy mode is `Prefix` + `[`.

1. From within, use `[space]` to enter copy mode.
2. Use the arrow keys to adjust the text to be selected.
3. Press `[enter]` to copy the selected text.

The default key to paste the text copied is `Prefix` + `]`.

> *Vi-like copy-paste*
>
> In your [config](#config), put this:
>
> ```
>     # Vi copypaste mode
>     set-window-option -g mode-keys vi
>     bind-key -t vi-copy 'v' begin-selection
>     bind-key -t vi-copy 'y' copy-selection
> ```

In addition to the "copy mode", tmux has advanced functionality to
programmatically copy and paste. Later in the book, the [Capturing pane content](#capture-pane)
section in the [Scripting tmux](#scripting-tmux) chapter goes into
`$ tmux capture-pane` and how you can use [targets](#targets) to copy pane
content into your paste buffer or files with `$ tmux save-buffer`.

## 小节

The server is one of the fundamental underpinnings of tmux. Initialized
automatically to the user, it persists by forking into the background. Running
behind the scenes, it ensures sessions, windows, panes, and buffers are
operating, even when the client is detached.

The server can hold one or more *sessions*. You can copy and paste between
sessions via the clipboard. In the next chapter, we will go deeper into the role
sessions play and how they help you organize and control your terminal
workspace.