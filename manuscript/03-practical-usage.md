# 开始使用（Practical usage） {#practical-usage}

好的让我们开始吧！打开 terminal 输入 `tmux` 按 回车键 enter。
```shell
    $ tmux
```
欢迎进入 tmux 的世界。

## 前缀 组合快捷键（ prefix key ）{#prefix-key}

我们通过带前缀的组合快捷键向 tmux 输命令。我们可以拆分窗口（windows），移动窗口，切换窗口，切换会话（sessions），或者自定义的命令。

这是一个我们必须克服的困难。

这有点像打街头霸王（[*Street Fighter*](https://en.wikipedia.org/wiki/Street_Fighter)）

在游戏中，玩家输入一套组合健，控制角色飞转踢脚和射击火球。玩家的游戏学习，变得更加习惯连击，他们凭直觉重复动作，发展出肌肉记忆。

不知道使用组合键使用tmux，你会死在水中的。

如果使用用Vim，Emacs或其他(Terminal User Interface，TUI) 终端应用程序。如果尚未将组合命令的概念内在化，那就现在开始吧。 在TUI / GUI应用程序中的组合命令的经验将有助于你理解。

记住一个组合键，就会减少一次手移开操作鼠标的操作。专注在短时记忆上，把工作一次性做完，减少错误的产生。



> ### 以前使用过 ``GNU Screen``？
>
> 通过 tmux 配置文件设置 tmux 的prefix key 。 在文件 `~/.tmux.conf`中设置 `prefix` :
> 
>```
>  set-option -g prefix C-a
>    ```
> 于是把prefix键设置成 `screen(1)` (另一个终端复用工具) 中的prefix key 。

默认的 leader prefix 是`<Ctrl-b>`。当按住`control`的时候，同时按`b`。


> ### 发送 tmux 命令组合键
>
> 训练:
>
> 1. 按下 `control` 键保持
> 2. 再按下 `b` 键保持
> 3. 同时释放两个键
>
> 多尝试几次。多做几次你就会觉得自然而然记住快捷键。
> 
>接下来，训练:
> 
>`<Ctrl-b> d` 组合键
> 
>1. 按下 `control`键保持
> 2. 按下 `b` 键保持
> 3. 同时释放两个键
> 4. 轻击打 `d`键
> 
>你刚刚发出了使用tmux的第一次组合键命令，看你跳出了tmux。

你已经断开和tmux session的连接。可通过`$ tmux attach` 再次连接上。


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
