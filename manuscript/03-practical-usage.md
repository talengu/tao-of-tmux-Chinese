# 开始使用（Practical usage） {#practical-usage}

好的让我们开始吧！打开 terminal 输入 `tmux` 按 回车键 enter。
```shell
    $ tmux
```
欢迎进入 tmux 的世界。

## 前缀 组合快捷键（ prefix key ）{#prefix-key}

我们通过带前缀的组合快捷键向 tmux 输命令。我们可以拆分窗口（windows），移动窗口，切换窗口，切换会话（sessions），或者自定义的命令。

这是一个我们必须克服的困难。

It's kind of like [*Street Fighter*](https://en.wikipedia.org/wiki/Street_Fighter).
In this video game, the player inputs a combination of buttons in sequence to
perform flying spinning kicks and shoot fireballs; sweet. As the player grows
more accustomed with the combos, they repeat moves by intuition, since they
develop muscle memory.

Without understanding how to send *command sequences* to tmux via the prefix
key, you'll be dead in the water.

Key sequences will come up later if you use Vim, Emacs, or other TUI (Terminal
User Interface) applications. If you haven't internalized the concept, let's do
it now. Prior experience command sequences in TUI/GUI applications will come in
handy.

When you memorize a key combo, it's one less time you'll be moving your hand
away from the keyboard to grab your mouse. You can focus your short-term memory
on getting stuff done, resulting in fewer mistakes.

> ### Coming from ``GNU Screen``?
>
> Your tmux prefix key can be set via your tmux configuration later!  In
> your `~/.tmux.conf` file, set the `prefix` option:
>
> ```
>     set-option -g prefix C-a
> ```
> This will set the prefix key to `screen(1)`'s (another terminal
> multiplexer's) prefix key.

The default leader prefix is `<Ctrl-b>`. While holding down the `control` key,
press `b`.

> ### Sending tmux commands
>
> Practice:
>
> 1. Press `control` key down and *hold it*.
> 2. Press `b` and hold it.
> 3. Release both keys at the same time.
>
> Try it a few times. It may feel unnatural until you've done it a couple
> times, which is normal when memorizing shortcuts.
>
> Now, let's try something:
>
> `<Ctrl-b> d`. So,
>
> 1. Press `control` key down and *hold it*.
> 2. Press `b` and hold it.
> 3. Release both keys at the same time.
> 4. Hit `d`!
>
> You've sent tmux your first command, and you're now outside of tmux!

You've detached the tmux session you were in. You can reattach via `$ tmux
attach`. 

### Nested tmux sessions

You can also send the prefix key to *nested* tmux sessions. For instance, if
you're inside a tmux client on a *local* machine and you SSH into a *remote* machine
in one of your panes, on the remote machine, you can attach the client via `tmux
attach` as you normally would. To send the prefix key to the machine's tmux
client, not your local one, hit the prefix key again.

So, if your prefix key is the default, `<Ctrl-b>`, do `<Ctrl-b>` + `b` again,
*then* hit the shortcut for what you want to do.

Example: If you wanted to create a window on the remote machine, which would normally
be `<Ctrl-b>` + `c` locally, it'd be `<Ctrl-b>` + `b` + `c`.

Hereinafter, the book will refer to shortcuts by `Prefix`. Instead
of `<Ctrl-b> + d`, you will see `Prefix` + `d`.

## Session persistence and the server model

If you use Linux or a similar system, you've likely brushed through [Job Control](https://en.wikipedia.org/wiki/Job_control_(Unix)),
such as [`fg(1)`](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/fg.html), [`jobs(1)`](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/jobs.html).
tmux behavior feels similar, like you ran `<Ctrl-z>` except, technically, you
were in a "job" all along. You were just using a client to view it.

Another way of understanding it: `<Ctrl-b>` + `d` closed the client connection,
therefore, 'detached' from the session.

Your tmux client disconnected from the server instance. The session, however, is
still running in the background.

## It's all commands

Multiple roads can lead you to the same behavior. *Commands* are what tmux uses
to define instructions for setting options, resizing, renaming, traversing,
switching modes, copying and pasting, and so forth.

- [Configs](#config) are the same as automatically running commands via
  `$ tmux command`.
- Internal tmux commands via `Prefix` + `:` prompt.
- Settings defined in your configuration can also set shortcuts, which can
  execute commands via keybindings via `bind-key`.
- Commands called from CLI via `$ tmux cmd`
- To pull it all together, [source code](#technical-stuff) files are prefixed
  `cmd-`.

## 小节

We've established tmux automatically creates a server upon starting it. The
server allows you to detach and later reattach your work. The keyboard
sequences you send to tmux require understanding how to send the prefix key.

Keyboard sequences, configuration, and command line actions all boil down to the
same core commands inside tmux.  In our next chapter, we will cover the server.
