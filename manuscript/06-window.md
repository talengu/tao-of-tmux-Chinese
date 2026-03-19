# 窗口（Windows） {#windows}

Windows 包含 [panes](#panes)。windows 又包含在 [session](#sessions)中。

它们还有[布局（layouts）](#window-layouts)，可以是许多预设尺寸中的一种，也可以是通过[窗格调整大小](#pane-resizing)定制的。

![](images/info/window.png)

你可以通过 tmux 底部的[状态栏](#status-bar)看到当前的窗口。

## Creating windows（创建窗口）

所有会话都至少从一个打开的窗口开始。从那里，你可以根据需要创建和关闭窗口。

窗口索引是 tmux 用来确定顺序的数字。第一个窗口的索引是 0，除非你通过配置中的 `base-index` 设置它。我通常在我的 tmux 配置中设置 `set -g base-index 1`，因为 0 在键盘上位于 9 之后。

`Prefix` + `c` 将在第一个空闲索引处创建一个新窗口。所以，如果你在第一个窗口，并且没有创建第二个窗口，它将创建第二个窗口。如果第二个窗口已经被占用，并且第三个窗口还没有创建，它将创建第三个窗口。

如果 `base_index` 是 1，并且已经创建了 7 个窗口，而第 5 个窗口缺失，创建新窗口将填补空闲的第 5 个索引，因为它是顺序中的下一个，没有什么填充它。下一个创建的窗口将是第八个。

## Naming windows（命名窗口）

就像会话一样，窗口也可以有名称。为它们贴标签有助于跟踪你在其中做什么。

![将窗口 'zsh' 重命名为 'renamed'](images/06-window/rename.png)

在 tmux 内部，最常用的是快捷键 `Prefix` + `,`。它会在 tmux 状态行中打开一个提示符，你可以在其中更改当前窗口的名称。

窗口的默认数字过一段时间也会成为肌肉记忆。但当你处于新的 tmux 流程中并想要组织自己时，命名会有所帮助。此外，如果你与其他用户共享 tmux，给出窗口内容的提示是很好的做法。

## Traversing windows（遍历窗口）

移动窗口有两种方式，首先通过 `Prefix` + `p` 和 `Prefix` + `n` 迭代，以及通过窗口索引，这可以直接带你到特定的窗口。

`Prefix` + `1`、`Prefix` + `2` 等等...允许通过它们的索引快速导航到窗口。与会改变的窗口名称不同，索引是一致的，只需要一个快速的组合键就可以调用。

提示输入窗口索引（对于大于 9 的索引很有用）使用 `Prefix` + `'`。如果窗口索引是 10 或以上，这会对你有很大帮助。

> ### 提示：搜索 + 遍历窗口查找文本
>
> 你可以通过 `Prefix` + `f` 转发到具有匹配文本字符串的窗口。

使用 `Prefix` + `l` 调出最后选择的窗口。

当前窗口列表可以通过 `Prefix` + `w` 显示。这也会给出窗口内的一些信息。在同时处理很多任务时很有帮助！

## Moving windows（移动窗口）

窗口也可以通过 `move-window` 及其相关快捷键逐个重新排序。如果一个窗口值得保持打开但不重要或很少查看，这很有帮助。移动窗口后，你可以随时继续重新排序它们。

命令 `$ tmux move-window` 可用于移动窗口。

接受的参数是 `-s`（你要移动的窗口）和 `-t`，你要移动到的目标位置。

你也可以使用短形式 `$ tmux movew`。

示例：将当前窗口移动到编号 2：

```shell
    $ tmux movew -t2
```
示例：将窗口 2 移动到窗口 1：

```shell
    $ tmux movew -s2 -t1
```
提示输入索引以将当前窗口移动到的快捷键是 `Prefix` + `.`。

## Layouts（布局）{#window-layouts}

`Prefix` + `space` 切换窗口*布局*。这些是自动调整[窗格](#panes)比例的预设配置。

截至 tmux 2.3，支持的布局有：

{width=75%}
![](images/06-window/even-horizontal.png)

{width=75%}
![](images/06-window/even-vertical.png)

{width=75%}
![](images/06-window/main-horizontal.png)

{width=75%}
![](images/06-window/main-vertical.png)

{width=75%}
![](images/06-window/tiled.png)

可以通过[调整窗格大小](#resizing-panes)进行具体调整。

要重置布局的比例（如在拆分或调整窗格大小后），你必须再次运行 `$ tmux select-layout` 来选择布局。

这与一些[平铺窗口管理器](https://en.wikipedia.org/wiki/Tiling_window_manager)的行为不同。例如，[*awesome*](https://awesomewm.org/) 和 [*xmonad*](http://xmonad.org/)会在新项目添加到其布局时自动处理比例。

为了在机器和终端尺寸上轻松重置为合理的布局，你可以在你的[配置](#config)中尝试这个：

```shell
bind m set-window-option main-pane-height 60\; select-layout main-horizontal
```

这允许你设置 `main-horizontal` 布局，并在每次执行 `Prefix` + `m` 时自动按比例设置底部窗格。

布局也可以自定义。要获取当前窗口的自定义布局片段，尝试这个：

```shell
$ tmux lsw -F "#{window_active} #{window_layout}" | grep "^1" | cut -d " " -f2
```

要应用这个布局：

```shell
$ tmux lsw -F "#{window_active} #{window_layout}" | grep "^1" | cut -d " " -f2
> 5aed,176x79,0,0[176x59,0,0,0,176x19,0,60{87x19,0,60,1,88x19,88,60,2}]

# resize your panes or try doing this in another window to see the outcome
$ tmux select-layout "5aed,176x79,0,0[176x59,0,0,0,176x19,0,60{87x19,0,60,1,88x19,88,60,2}]"
```

## Closing windows（关闭窗口）

有两种方式可以关闭窗口。首先，退出或杀死窗口中的每个窗格。窗格可以通过 `Prefix` + `x` 或在窗格的 shell 中按 `Ctrl` + `d` 来杀死。第二种方式，`Prefix` + `&`，会提示你是否真的要删除窗口。警告：这将销毁窗口的所有窗格以及其中的进程。

在当前窗口内，尝试这个：

```shell
$ tmux kill-window
```

另一件事，当[编写脚本](#scripting-tmux)或从外部尝试杀死窗口时，使用窗口索引的[目标](#targets)：

```shell
$ tmux kill-window -t2
```
如果你正在尝试找到要杀死的窗口的目标，它们位于[状态行](#status-line)的中间部分的数字中，也可以通过 `$ tmux choose-window`。在 choose-window 中后，按"return"可以回到你之前的位置。

## 小节（Summary）

在本章中，你学会了如何通过重命名和更改布局来操作窗口，以及在紧急情况下或在 shell 编写 tmux 脚本时杀死窗口的几种方法。此外，本章还演示了如何通过打印 `window_layout` 模板变量来保存任何 tmux 布局。

如果你在 tmux 会话中，你总是至少有一个窗口打开，你会在其中。而窗口内会有"窗格"；一个 shell 中的 shell。当窗口关闭其所有窗格时，窗口也会关闭。在下一章中，我们将更深入地探讨窗格。
