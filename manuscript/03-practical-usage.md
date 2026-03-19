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
> 接下来，训练:
> 
> `<Ctrl-b> d` 组合键
> 
> 1. 按下 `control`键保持
> 2. 按下 `b` 键保持
> 3. 同时释放两个键
> 4. 轻击打 `d`键
> 
> 你刚刚发出了使用tmux的第一次组合键命令，看你跳出了tmux。

你已经断开和tmux session的连接。可通过`$ tmux attach` 再次连接上。

### 嵌套的 tmux 会话（Nested tmux sessions）

你也可以将前缀键发送到*嵌套*的 tmux 会话中。例如，如果你在*本地*机器上的 tmux 客户端中，并且通过 SSH 连接到*远程*机器的一个窗格，在远程机器上，你可以像往常一样通过 `tmux attach` 附加客户端。要将前缀键发送到机器的 tmux 客户端，而不是你本地的客户端，需要再次按下前缀键。

因此，如果你的前缀键是默认的 `<Ctrl-b>`，就再按一次 `<Ctrl-b>` + `b`，*然后*再按你想执行的快捷键。

例如：如果想在远程机器上创建一个窗口，通常本地是 `<Ctrl-b>` + `c`，那么在远程机器上就是 `<Ctrl-b>` + `b` + `c`。

此后，本书将通过 `Prefix` 来引用快捷键。你会看到 `Prefix` + `d` 而不是 `<Ctrl-b> + d`。

## Session persistence and the server model（会话持久化和服务器模型）

如果你使用 Linux 或类似的系统，你可能已经浏览过 [Job Control（作业控制）](https://en.wikipedia.org/wiki/Job_control_(Unix))，比如 [`fg(1)`](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/fg.html)、[`jobs(1)`](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/jobs.html)。
tmux 的行为感觉类似，就像你运行了 `<Ctrl-z>` 一样，只是从技术上讲，你一直处于一个"作业"中。你只是在使用一个客户端来查看它。

另一种理解方式是：`<Ctrl-b>` + `d` 关闭了客户端连接，因此"脱离"了会话。

你的 tmux 客户端与服务器实例断开了连接。然而，会话仍在后台运行。

## It's all commands（都是命令）

多条路径可以让你达到相同的行为。*命令*是 tmux 用来定义设置选项、调整大小、重命名、遍历、切换模式、复制粘贴等指令的方式。

- [配置文件（Configs）](#config) 相当于通过 `$ tmux command` 自动运行命令。
- 通过 `Prefix` + `:` 提示符使用内部 tmux 命令。
- 配置文件中定义的设置也可以设置快捷键，这些快捷键可以通过 `bind-key` 执行命令。
- 通过 CLI 调用命令：`$ tmux cmd`
- 综合来看，[源代码](#technical-stuff) 文件以 `cmd-` 开头。

## 小节

我们已经确定 tmux 在启动时会自动创建一个服务器。服务器允许你脱离然后重新附加你的工作。你发送给 tmux 的键盘序列需要理解如何发送前缀键。

键盘序列、配置和命令行操作最终都归结为 tmux 内部的相同核心命令。在下一章中，我们将介绍服务器。
